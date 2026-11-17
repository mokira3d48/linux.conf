# Remote Development over SSH with SSHFS

Nov 17, 2026 · @Dr Mokira

## Overview

This guide lets you work on a folder that lives on another machine (the **remote machine**) from your own computer (the **local machine**), without typing a password every time. At the end, the remote folder appears on your computer like a normal folder, so you can open it in VS Code.

The examples use these values. Replace them with your own:

| What | Example value |
| --- | --- |
| Remote user | `uatm` |
| Remote IP address | `172.16.0.106` |
| SSH port | `22000` |
| Short name (alias) | `uatm` |
| Remote project folder | `/home/uatm/project` |

Commands marked **(remote)** run on the remote machine. Commands marked **(local)** run on your computer. Local commands assume Linux (Debian/Ubuntu).

## Step 1: Install the SSH server (remote)

The remote machine needs an SSH server so your computer can connect to it. Skip this step if you can already connect with `ssh`.

1. Install and start the server:

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable --now ssh
```

2. Check that it is running:

```bash
sudo systemctl status ssh
```

You should see `active (running)`.

3. (Optional) To use a port other than 22, edit `/etc/ssh/sshd_config`, set `Port 22000`, then restart with `sudo systemctl restart ssh`. If a firewall is active, open the port with `sudo ufw allow 22000/tcp`.
4. Test from your computer **(local)**:

```bash
ssh -p 22000 uatm@172.16.0.106
```

Type `yes` the first time, then your password. Type `exit` to leave.

## Step 2: Create an SSH key (local)

An SSH key replaces your password. It has two parts: a **private key** that stays on your computer, and a **public key** that you give to the remote machine.

1. Check whether you already have one:

```bash
ls ~/.ssh/
```

If you see `id_ed25519` and `id_ed25519.pub`, go to Step 3.

2. Otherwise, create one:

```bash
ssh-keygen -t ed25519
```

Press Enter to accept the default location. You can then set a **passphrase** (a password for the key). It is recommended; Step 5 makes sure you only type it once per session.

Never share the private key (`id_ed25519`, without `.pub`).

## Step 3: Copy the public key to the remote machine (local)

This tells the remote machine to trust your key. You type the remote password one last time.

```bash
ssh-copy-id -p 22000 -i ~/.ssh/id_ed25519.pub uatm@172.16.0.106
```

Then make sure the permissions on the remote machine are correct. SSH ignores the key if they are too open:

```bash
ssh -p 22000 uatm@172.16.0.106 "chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys && chmod go-w ~"
```

## Step 4: Configure \~/.ssh/config (local)

`~/.ssh/config` is a text file on **your computer**. It gives the remote machine a short name, so you can type `ssh uatm` instead of the full address, port and key.

1. Open (or create) the file:

```bash
nano ~/.ssh/config
```

2. Paste this content and adapt the values:

```
Host uatm
    HostName 172.16.0.106
    User uatm
    Port 22000
    IdentityFile ~/.ssh/id_ed25519

Host *
    AddKeysToAgent yes
```

What each line means:

- `Host uatm`: the short name you choose.
- `HostName`: the real IP address or name of the remote machine.
- `User`: your username on the remote machine.
- `Port`: the SSH port (22 if you leave this line out).
- `IdentityFile`: the private key to use.
- `Host *` + `AddKeysToAgent yes`: for every connection, the key is added to the SSH agent the first time you unlock it, so you do not have to type the passphrase again.

3. Save (`Ctrl+O`, Enter, `Ctrl+X`) and protect the file:

```bash
chmod 600 ~/.ssh/config
```

## Step 5: Load the key and test (local)

If your key has a passphrase, programs that cannot ask for it (VS Code, Claude Code, scripts) will fail. Loading the key into the SSH agent solves this.

1. Load the key (type the passphrase once):

```bash
ssh-add ~/.ssh/id_ed25519
```

With `AddKeysToAgent yes` from Step 4, a normal `ssh uatm` also does this automatically. Repeat after each reboot.

2. Test a connection that is not allowed to ask for anything:

```bash
ssh -o BatchMode=yes uatm "echo OK"
```

If it prints `OK`, passwordless login works. Otherwise, see Troubleshooting.

Start VS Code from the same desktop session, so it can use the agent.

## Step 6: Install SSHFS (local)

SSHFS shows a remote folder as if it were a folder on your computer. Nothing needs to be installed on the remote machine.

```bash
sudo apt install sshfs
```

On macOS, install macFUSE and SSHFS from [macfuse.github.io](https://macfuse.github.io). On Windows, install WinFsp, then SSHFS-Win.

## Step 7: Mount and use the remote folder (local)

1. Create an empty local folder (the **mount point**):

```bash
mkdir -p ~/remote
```

2. Mount the remote folder into it. Thanks to Step 4, `uatm` already includes the IP, user, port and key:

```bash
sshfs uatm:/home/uatm/project ~/remote -o reconnect,ServerAliveInterval=15
```

`reconnect` and `ServerAliveInterval` reconnect automatically if the network drops.

3. Check that it works:

```bash
ls ~/remote
```

You should see the remote files.

4. Open it in VS Code: **File > Open Folder** and choose `~/remote`, or run `code ~/remote`. Every file you save is written directly on the remote machine.
5. Run commands on the remote machine through SSH (builds, tests, git), because running them on the mounted folder is slow:

```bash
ssh uatm "cd /home/uatm/project && <command>"
```

To let Claude Code do this, add a `CLAUDE.md` file at the root of the folder:

```markdown
This project lives on a remote machine.
Never run build, test or git commands locally.
Always run them through SSH:
ssh uatm "cd /home/uatm/project && <command>"
```

6. Unmount when you are done (close VS Code first):

```bash
fusermount -u ~/remote
```

On macOS, use `umount ~/remote`.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| `Permission denied (publickey,password)` with `BatchMode=yes` | The key has a passphrase and is not loaded. Run `ssh-add ~/.ssh/id_ed25519` (Step 5). |
| Still asks for a password | Redo Step 3 and the `chmod` command. Run `ssh -v uatm` and look for `Server accepts key`. |
| `Connection refused` | Wrong port, or the SSH server is not running. Check Step 1. |
| `fusermount: mountpoint is not empty` | Use an empty folder as the mount point. |
| `Transport endpoint is not connected` | The connection dropped. Run `fusermount -u ~/remote`, then mount again. |
| VS Code is slow on the mounted folder | Large folders (`node_modules`, datasets) are slow over SSHFS. Run heavy commands with `ssh uatm "..."`. |
