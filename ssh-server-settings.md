Here's a comprehensive guide to configuring an SSH server with security best practices on Linux:

## **1. Install SSH Server**

**Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install openssh-server
```

**CentOS/RHEL/Fedora:**
```bash
sudo yum install openssh-server
# or for newer versions
sudo dnf install openssh-server
```

**Enable and start service:**
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

---

## **2. Secure SSH Configuration**

Edit the SSH configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```

### **Essential Security Settings:**

```bash
# Disable root login
PermitRootLogin no

# Change default port (optional but recommended)
Port 2222  # Change from default 22

# Limit user access
AllowUsers username1 username2
# or
AllowGroups ssh-users

# Disable password authentication (use key-based)
PasswordAuthentication no

# Enable key authentication
PubkeyAuthentication yes

# Disable empty passwords
PermitEmptyPasswords no

# Protocol settings
Protocol 2  # Only use SSHv2

# Key exchange algorithms
KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group16-sha512

# Ciphers
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com

# MAC algorithms
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com

# Login grace time
LoginGraceTime 60

# Max authentication attempts
MaxAuthTries 3

# Client alive intervals
ClientAliveInterval 300
ClientAliveCountMax 2

# Disable X11 forwarding (unless needed)
X11Forwarding no

# Restrict SFTP users (if needed)
# Subsystem sftp internal-sftp
# Match Group sftpusers
#    ChrootDirectory /home/%u
#    ForceCommand internal-sftp
#    AllowTcpForwarding no
```

---

## **3. Key-Based Authentication Setup**

### **Generate SSH Key Pair (on client):**
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# or for older systems
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### **Copy Public Key to Server:**
```bash
ssh-copy-id -p 2222 username@server_ip
# or manually:
cat ~/.ssh/id_ed25519.pub | ssh -p 2222 username@server_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### **Secure Key Permissions:**
```bash
# On server:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Connect to ssh server:

```bash
ssh -p 2222 username@server_ip
```

To copy file into ssh server:

```bash
rsync -avzP -e "ssh -p 2222" <source_file> username@192.168.100.18:<dest_file>
```

---

## **4. Firewall Configuration**

**UFW (Ubuntu):**
```bash
sudo ufw allow 2222/tcp
sudo ufw deny 22/tcp  # Block default port
sudo ufw enable
```

**Firewalld (CentOS/RHEL):**
```bash
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --permanent --remove-service=ssh
sudo firewall-cmd --reload
```

**iptables:**
```bash
sudo iptables -A INPUT -p tcp --dport 2222 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j DROP
```

---

## **5. Additional Security Measures**

### **Fail2ban Installation:**
```bash
sudo apt install fail2ban  # Ubuntu/Debian
sudo yum install fail2ban  # CentOS/RHEL
```

**Configure Fail2ban for SSH:**
```bash
sudo nano /etc/fail2ban/jail.local
```

Add:
```ini
[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
```

Restart fail2ban:
```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

### **Two-Factor Authentication (Optional):**
```bash
sudo apt install libpam-google-authenticator
```

Edit `/etc/pam.d/sshd`:
```
auth required pam_google_authenticator.so
```

Edit `/etc/ssh/sshd_config`:
```
AuthenticationMethods publickey,keyboard-interactive
```

---

## **6. User and Access Management**

**Create SSH-only group:**
```bash
sudo groupadd ssh-users
sudo usermod -a -G ssh-users username
```

In `sshd_config`:
```
AllowGroups ssh-users
```

**Configure user-specific settings:**
```
Match User username
    AllowTcpForwarding no
    PermitTTY no
    ForceCommand /bin/rbash
```

---

## **7. Monitoring and Logging**

**Enable verbose logging:**
```
LogLevel VERBOSE
```

**Monitor SSH connections:**
```bash
# Check active connections
ss -tunlp | grep :2222
netstat -tulpn | grep :2222

# View logs
sudo tail -f /var/log/auth.log
sudo journalctl -u ssh -f
```

---

## **8. Complete Configuration Example**

Here's a secure `sshd_config` template:

```bash
# /etc/ssh/sshd_config
Port 2222
Protocol 2
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key

# Authentication
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
PermitEmptyPasswords no

# Key exchange, ciphers, and MACs
KexAlgorithms curve25519-sha256@libssh.org
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
MACs hmac-sha2-512-etm@openssh.com

# Session settings
ClientAliveInterval 300
ClientAliveCountMax 2
MaxAuthTries 3
LoginGraceTime 60

# Access control
AllowUsers username1 username2
# AllowGroups ssh-users

# Other security
X11Forwarding no
PrintMotd no
IgnoreRhosts yes
UsePAM yes

# Logging
LogLevel VERBOSE
```

---

## **9. Testing and Validation**

**Test configuration before restarting:**
```bash
sudo sshd -t
```

**Reload SSH service:**
```bash
sudo systemctl reload ssh
# or
sudo systemctl restart ssh
```

**Test connection from another terminal:**
```bash
ssh -p 2222 username@server_ip
```

**Keep a backup session open** in case you get locked out!

---

## **10. Regular Maintenance**

**Update regularly:**
```bash
sudo apt update && sudo apt upgrade openssh-server
```

**Monitor logs:**
```bash
sudo grep "Failed password" /var/log/auth.log
sudo fail2ban-client status sshd
```

**Rotate host keys periodically:**
```bash
sudo ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
sudo systemctl restart ssh
```

---

## **Important Notes:**

- **Always test changes** in a non-production environment first
- **Keep a backup SSH session** active when making changes
- **Consider using non-standard ports** to reduce automated attacks
- **Regularly update SSH** and the underlying OS
- **Monitor logs** for suspicious activity
- **Use fail2ban** to block brute force attacks

This configuration provides a solid foundation for a secure SSH server while maintaining functionality! 🔒
