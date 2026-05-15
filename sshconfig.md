## Change SSH Key Password

To change the passphrase (password) of your SSH private key, use the `ssh-keygen` tool that comes with OpenSSH. The passphrase only protects the private key file on your disk; changing it does **not** alter the actual key pair, so your existing public key on servers remains valid.

---

### Interactive method (simple)
```bash
ssh-keygen -p -f ~/.ssh/id_ed25519
```
- Replace `id_ed25519` with your key filename (e.g., `id_rsa`, `id_ecdsa`).
- You’ll be prompted for the **current** passphrase (if any), then asked to enter and confirm the **new** one.
- If you want to **remove** the passphrase, just press Enter twice when asked for the new one (leaving it empty).

If your key is in a non‑default location, use its full path:
```bash
ssh-keygen -p -f /path/to/private_key
```

---

### Non‑interactive (script‑friendly) method
You can supply the old and new passphrases directly on the command line:
```bash
ssh-keygen -p -f ~/.ssh/id_rsa -P "old_passphrase" -N "new_passphrase"
```
- `-P` : **old** passphrase (use `-P ""` if the key currently has no passphrase).  
- `-N` : **new** passphrase (use `-N ""` to remove it).

**Security note:** Command‑line passphrases may be visible in process listings. Prefer the interactive method on shared machines.

---

### Before you start – backup your key
Mistakes (e.g., a typo in the new passphrase) can lock you out. Make a copy first:
```bash
cp ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.backup
```

---

### What if I forgot my current passphrase?
There is no way to recover or change a passphrase without knowing the old one. You must:
1. Generate a new key pair: `ssh-keygen -t ed25519 -C "your_email@example.com"`
2. Replace your public key on every server/account where it was installed.

---

### Works for all OpenSSH key types
The commands above work for RSA, Ed25519, ECDSA, and DSA keys. If you have a very old PEM‑formatted key (header `-----BEGIN RSA PRIVATE KEY-----`), you may need to convert it first:
```bash
ssh-keygen -p -m PEM -f old_key
```
