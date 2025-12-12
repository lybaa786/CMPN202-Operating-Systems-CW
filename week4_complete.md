# Week 4: Initial System Configuration & Security Implementation

<!-- 🎬 DEMO NOTE: In your video, say:
"In Week 4, I implemented foundational security controls. I configured SSH key-based authentication, set up a firewall to restrict access to my workstation only, and created a non-root administrative user. Let me demonstrate each of these live." (20 seconds) -->

## Overview

This week focused on implementing foundational security controls identified in Week 2. All configurations were performed via SSH from my workstation, demonstrating remote administration proficiency (LO4). The three core security implementations are:

1. SSH key-based authentication (replacing password authentication)
2. Firewall configuration (UFW) restricting SSH to workstation IP only
3. User and privilege management (non-root administrative user)

---

## 1. SSH Key-Based Authentication

<!-- 🎬 DEMO NOTE: Show terminal and say:
"First, I generated an SSH key pair on my workstation. The private key stays on my computer, and I copied the public key to the server. Now I can login without a password, using cryptographic authentication instead." (20 seconds) -->

<!-- WHAT THIS IS: Instead of typing a password every time, SSH keys use cryptography. Your computer has a "private key" (secret) and the server has a "public key" (not secret). Only your private key can prove you own the public key. WAY more secure than passwords. -->

### Step 1: Generate SSH Key Pair on Workstation

On my workstation (NOT on the server), I generated an SSH key pair:

```bash
# On workstation (Windows/Mac/Linux)
ssh-keygen -t ed25519 -C "vboxuser@OSCM"
```

**Command explanation:**
- `ssh-keygen`: Tool to create SSH key pairs
- `-t ed25519`: Use Ed25519 algorithm (modern, secure, fast)
- `-C "comment"`: Adds a label to identify this key

**Output:**
```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519): [Press Enter]
Enter passphrase (empty for no passphrase): [Type passphrase or press Enter]
Enter same passphrase again: [Type again or press Enter]
Your identification has been saved in /home/user/.ssh/id_ed25519
Your public key has been saved in /home/user/.ssh/id_ed25519.pub
```

**Security decision: Passphrase**
- ✅ **With passphrase:** More secure (protects key if laptop stolen), but less convenient
- ❌ **Without passphrase:** Less secure, but convenient for automation
- **My choice:** [WITH PASSPHRASE / WITHOUT PASSPHRASE] because [your reason]

### Step 2: Copy Public Key to Server

```bash
# Method 1: Using ssh-copy-id (easiest)
ssh-copy-id vboxuser@192.168.56.101

# Method 2: Manual copy (if ssh-copy-id not available)
cat ~/.ssh/id_ed25519.pub | ssh vboxuser@192.168.56.101 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

**What this does:**
- Reads your public key from workstation
- Connects to server via SSH
- Creates `.ssh` directory if it doesn't exist
- Appends public key to `~/.ssh/authorized_keys` file

### Step 3: Test Key-Based Authentication

```bash
# Try connecting - should NOT ask for password
ssh vboxuser@192.168.56.101
```

**Expected result:** Connection succeeds without password prompt (or only asks for key passphrase if you set one)

![SSH Key Login Success](week4-ssh-key-login.png)

### Step 4: Disable Password Authentication

**IMPORTANT:** Only do this AFTER confirming key-based login works!

Edit SSH server configuration:
```bash
sudo nano /etc/ssh/sshd_config
```

**Changes to make:**
```bash
# Find these lines and change them:
PasswordAuthentication no          # Was: yes
PubkeyAuthentication yes           # Should already be yes
PermitRootLogin no                 # Was: yes or prohibit-password
```

**Before (less secure):**
```
PasswordAuthentication yes
PermitRootLogin yes
```

**After (more secure):**
```
PasswordAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
```

Save file (Ctrl+X, then Y, then Enter)

Restart SSH service:
```bash
sudo systemctl restart ssh
```

**Verify it worked:**
```bash
sudo systemctl status ssh
# Should show "active (running)"
```

### Step 5: Test from New Terminal

Open a NEW terminal and try connecting:
```bash
ssh vboxuser@192.168.56.101
```

Should work with key, but password login should now be impossible.

![SSH Configuration After Changes](week4-ssh-config-after.png)

**Security improvement:**
- ✅ Password brute-force attacks now impossible
- ✅ Only my workstation (with private key) can access server
- ✅ Root cannot login directly via SSH

---

## 2. Firewall Configuration (UFW)

<!-- 🎬 DEMO NOTE: Show terminal and say:
"Next, I configured UFW firewall to block all incoming connections except SSH from my workstation IP only. This means even if someone gets on my home network, they can't access the server." (20 seconds) -->

<!-- WHAT THIS IS: UFW (Uncomplicated Firewall) is Ubuntu's firewall. It blocks network traffic based on rules. We're setting it to block everything except SSH from your computer's IP. -->

### Step 1: Check UFW Status

```bash
sudo ufw status
```

**Expected output:**
```
Status: inactive
```

### Step 2: Set Default Policies

```bash
# Block all incoming traffic by default
sudo ufw default deny incoming

# Allow all outgoing traffic (server can reach internet)
sudo ufw default allow outgoing
```

**Why these defaults?**
- **Deny incoming:** Secure by default, only allow what we explicitly need
- **Allow outgoing:** Server needs to download updates, packages, etc.

### Step 3: Allow SSH from Workstation IP ONLY

**CRITICAL:** My workstation IP is `192.168.56.1` (or check with `ip addr` on host)

```bash
# Allow SSH only from workstation
sudo ufw allow from 192.168.56.1 to any port 22

# Alternatively, if you want to allow from entire host-only network:
# sudo ufw allow from 192.168.56.0/24 to any port 22
```

**What this rule means:**
- `from 192.168.56.1`: Only traffic originating from this IP
- `to any`: To any interface on this server
- `port 22`: On SSH port 22

**Important:** DO NOT just run `sudo ufw allow 22` - that allows SSH from ANYWHERE!

### Step 4: Enable UFW

```bash
sudo ufw enable
```

**Warning message:**
```
Command may disrupt existing ssh connections. Proceed with operation (y|n)?
```

Type `y` and press Enter.

**Why the warning?** If you enable UFW without allowing SSH, you'll lock yourself out!

### Step 5: Verify Firewall Rules

```bash
sudo ufw status verbose
```

**Expected output:**
```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22                         ALLOW IN    192.168.56.1
```

![UFW Status](week4-ufw-status.png)

### Step 6: Test Firewall Rules

**Test 1: SSH should still work from your workstation**
```bash
# From workstation
ssh vboxuser@192.168.56.101
# Should connect successfully
```

**Test 2: Verify only your IP is allowed**
```bash
sudo ufw status numbered
```

This shows all rules with numbers for reference.

### Firewall Configuration File Comparison

**Before UFW enabled:**
```bash
# No firewall active
sudo iptables -L
# Shows: Policy ACCEPT (all traffic allowed)
```

**After UFW enabled:**
```bash
sudo iptables -L
# Shows: Complex rules blocking traffic except SSH from 192.168.56.1
```

**Security improvement:**
- ✅ All incoming connections blocked except SSH
- ✅ SSH only accessible from my workstation (192.168.56.1)
- ✅ Port scanning from other IPs shows "filtered" (hidden)
- ✅ Outgoing connections still work (for updates)

---

## 3. User and Privilege Management

<!-- 🎬 DEMO NOTE: Say:
"I created a non-root administrative user with sudo privileges. This follows the principle of least privilege—I use a regular account for daily tasks and only elevate to admin when needed." (15 seconds) -->

<!-- WHAT THIS IS: Running everything as "root" (admin) is dangerous. If you make a mistake, you can break the whole system. Instead, create a regular user and use 'sudo' only when you need admin powers. -->

### Step 1: Create New Administrative User

**Note:** You might already have `vboxuser` as your main user. This step shows how to create ANOTHER user if needed, or you can document your existing user setup.

```bash
# Create new user
sudo adduser adminuser

# You'll be prompted:
# Enter new UNIX password: [type password]
# Retype new UNIX password: [type again]
# Full Name []: Admin User
# Room Number []: [press Enter]
# Work Phone []: [press Enter]
# Home Phone []: [press Enter]
# Other []: [press Enter]
# Is the information correct? [Y/n] Y
```

### Step 2: Add User to Sudo Group

```bash
# Grant sudo privileges
sudo usermod -aG sudo adminuser

# Verify user is in sudo group
groups adminuser
# Output should include: adminuser : adminuser sudo
```

**What this does:**
- `-aG sudo`: **a**ppends user to **G**roup "sudo"
- Users in sudo group can run administrative commands with `sudo` prefix

### Step 3: Test Sudo Access

```bash
# Switch to new user
su - adminuser

# Try sudo command
sudo apt update
# Should prompt for adminuser's password, then run

# Exit back to original user
exit
```

### Step 4: Configure Sudo Timeout (Optional)

Edit sudo configuration:
```bash
sudo visudo
```

Add this line:
```
Defaults timestamp_timeout=5
```

**What this does:** After using sudo, you must re-enter password after 5 minutes of inactivity (default is 15 minutes)

Save and exit (Ctrl+X, Y, Enter)

### Current User Configuration

**List all users:**
```bash
cat /etc/passwd | grep -E "vboxuser|adminuser"
```

**Output:**
```
vboxuser:x:1000:1000:VBox User:/home/vboxuser:/bin/bash
adminuser:x:1001:1001:Admin User:/home/adminuser:/bin/bash
```

**Sudo group members:**
```bash
getent group sudo
```

**Output:**
```
sudo:x:27:vboxuser,adminuser
```

### Privilege Management Best Practices Implemented

✅ **Non-root account for daily operations**
- All work done as `vboxuser`, not root

✅ **Sudo for administrative tasks**
- Must explicitly use `sudo` for admin commands
- Creates audit trail in `/var/log/auth.log`

✅ **Root login disabled**
- Cannot SSH as root (configured in Step 1)
- Cannot `su` to root without password

✅ **Sudo timeout configured**
- Must re-authenticate after inactivity

**Security improvement:**
- ✅ Reduced attack surface (root not directly accessible)
- ✅ Audit trail of all administrative actions
- ✅ Mistakes less catastrophic (sudo prompts for confirmation)
- ✅ Follows principle of least privilege

---

## 4. Evidence Summary

<!-- 🎬 DEMO NOTE: Show these screenshots in sequence:
"Here's proof everything is configured correctly. SSH key authentication working, firewall blocking unauthorized access, and proper user privileges in place." (15 seconds) -->

### SSH Key Authentication Evidence

![SSH Key Login](week4-ssh-key-login.png)

**Demonstrates:**
- Successful SSH connection without password
- Command prompt shows `vboxuser@OSCM`
- Connection using private key authentication

### SSH Configuration Files

**Before hardening:**
```bash
PasswordAuthentication yes
PermitRootLogin yes
```

**After hardening:**
```bash
PasswordAuthentication no
PubkeyAuthentication yes
PermitRootLogin no
```

![SSH Config Comparison](week4-sshd-config.png)

### Firewall Rules

```bash
sudo ufw status verbose
```

![UFW Status Verbose](week4-ufw-verbose.png)

**Shows:**
- UFW enabled and active
- Default deny incoming
- SSH allowed only from 192.168.56.1
- Logging enabled

### User and Privilege Configuration

```bash
# Current user
whoami

# Sudo access test
sudo apt update

# User groups
groups
```

![User Privileges Test](week4-user-privileges.png)

### Remote Administration Evidence

All configurations performed via SSH:

![SSH Session During Configuration](week4-ssh-remote-config.png)

**Terminal prompt shows:** `vboxuser@OSCM:~$` confirming remote execution

---

## 5. Challenges Encountered

### Challenge 1: UFW Lockout Risk

**Problem:** If I enabled UFW before allowing SSH, I would lock myself out of the server.

**Prevention:** Added SSH allow rule BEFORE enabling UFW:
```bash
sudo ufw allow from 192.168.56.1 to any port 22
sudo ufw enable
```

**Lesson learned:** Always configure firewall rules BEFORE enabling the firewall when working remotely. Once locked out, would need VirtualBox console access to fix.

### Challenge 2: SSH Key Permissions

**Problem:** SSH keys must have correct file permissions, or SSH refuses to use them.

**Solution:**
```bash
# On workstation
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub

# On server
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

**Lesson learned:** SSH is security-sensitive and enforces strict permission requirements. Too permissive = security risk, SSH won't use the keys.

### Challenge 3: Testing Password Disable

**Problem:** Needed to verify password authentication was truly disabled without locking myself out.

**Solution:** Kept one SSH session open while testing from a second terminal. If second terminal failed to connect, I still had the first session to fix issues.

**Best practice:** Never close your last SSH session until you've confirmed new authentication works.

---

## 6. Security Baseline Status

Checklist progress from Week 2:

### SSH Hardening
- ✅ SSH key-based authentication enabled
- ✅ Password authentication disabled
- ✅ Root login via SSH disabled
- ✅ SSH access restricted to workstation IP (via firewall)
- ⏳ SSH port change (optional - not implemented)
- ⏳ Connection timeouts (will configure in Week 5)

### Firewall Configuration
- ✅ UFW enabled
- ✅ Default deny incoming
- ✅ Default allow outgoing
- ✅ SSH allowed only from 192.168.56.1
- ✅ UFW logging enabled
- ⏳ Application-specific ports (Week 6 after app testing)

### User Privilege Management
- ✅ Non-root administrative user created
- ✅ User added to sudo group
- ✅ Root login disabled
- ✅ Sudo timeout configured
- ✅ User accounts reviewed

### Not Yet Implemented (Week 5)
- ⏳ AppArmor configuration review
- ⏳ Automatic security updates
- ⏳ fail2ban installation
- ⏳ Security baseline verification script
- ⏳ Remote monitoring script

---

## 7. Learning Reflections

**Key Learnings:**

**SSH Key Authentication is Superior:**
Password authentication is vulnerable to brute-force attacks, even with strong passwords. Key-based authentication uses 2048-4096 bit keys that are computationally infeasible to brute-force. The convenience cost (managing keys) is worth the security benefit.

**Firewall Rule Order Matters:**
UFW processes rules in order. Specific rules (allow from specific IP) should come before general rules. Understanding rule precedence prevents misconfigurations.

**Remote Administration Requires Caution:**
All changes were made via SSH. One mistake (e.g., enabling firewall without allowing SSH) would lock me out, requiring VirtualBox console access. This emphasizes the importance of testing and maintaining backup access.

**Defense in Depth Validation:**
Even with SSH keys, the firewall provides additional protection. If my private key were stolen, the firewall still blocks access unless the attacker is on my workstation IP. Multiple security layers provide redundancy.

**Principle of Least Privilege:**
Using a non-root account with sudo is more secure than running as root. Every `sudo` command is logged, creating an audit trail. This follows professional security best practices.

---

## References

[1] OpenSSH Project, "OpenSSH Key Management," OpenBSD Foundation, 2024. [Online]. Available: https://www.openssh.com/manual.html [Accessed: Dec. 12, 2024]

[2] Ubuntu Documentation, "UFW - Uncomplicated Firewall," Canonical Ltd., 2024. [Online]. Available: https://help.ubuntu.com/community/UFW [Accessed: Dec. 12, 2024]

[3] Red Hat, "Sudo Configuration Best Practices," Red Hat Enterprise Linux Documentation, 2024. [Online]. Available: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-sudo-access_configuring-basic-system-settings [Accessed: Dec. 12, 2024]

---

**Last Updated:** [TODAY'S DATE]

---

[← Back to Week 3](#) | [Forward to Week 5 →](#)