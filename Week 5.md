Phase 5: Advanced Security and Monitoring Infrastructure

1. Implementing Access Control using AppArmo:
- AppArmor restricts the program behaviour and improves the ystem security.
- Each application has a profile that defines what it is allowed to do. These profiles control things like file access, permissions, and network use.
- AppArmor runs in either Enforce mode (blocks violations) or Complain mode (only logs them).

Step 1: Verifying AppArmor is Running by checking which system is active:
<img width="1817" height="543" alt="image" src="https://github.com/user-attachments/assets/fd98c0f6-5071-44d1-b639-2787d6f9c23b" />

Step 2: Review Active Profiles:
<img width="773" height="929" alt="image" src="https://github.com/user-attachments/assets/92fbf3fa-3276-487b-a8a4-4edffcfbefc2" />

Step 3: Checking SSH Profile Specifically:

OUTPUT: cat: /etc/apparmor.d/usr.sbin.sshd: No such file or directory
This happens because Ubuntu 24.04 does not include a separate AppArmor profile for SSH by default. This is normal and does not mean SSH is insecure.

To verify that SSH is still protected: 
sudo aa-status | grep ssh

<img width="947" height="127" alt="image" src="https://github.com/user-attachments/assets/d868e109-8520-4ab7-9ab4-27887df159d5" />

This confirms that no SSH profile is listed, which is expected on Ubuntu 24.04. SSH is still protected using other security mechanisms such as systemd hardening and kernel-level protections.

Step 4: Documenting the AppArmor Configuration:
<img width="881" height="657" alt="image" src="https://github.com/user-attachments/assets/958fc3a1-02f6-4ba8-9f0e-b24fa3dca408" />
<img width="644" height="902" alt="image" src="https://github.com/user-attachments/assets/04e354d8-dea2-4065-9173-9f6fa56f6159" />
<img width="316" height="940" alt="image" src="https://github.com/user-attachments/assets/62faac3e-5ffa-4130-893c-31b568d1c961" />
<img width="347" height="986" alt="image" src="https://github.com/user-attachments/assets/4870d99c-336f-4a69-b1ab-84e64963ac64" />
<img width="1478" height="500" alt="image" src="https://github.com/user-attachments/assets/d9beea03-5ac0-41cb-a8f6-a29a1550e9ca" />

*Conclusion*:
<img width="1553" height="329" alt="image" src="https://github.com/user-attachments/assets/413c70d2-c937-41b7-a938-cc48719550d4" />


2. Configuring automatic security updates:

Step 1: Installing Unattended-Upgrades:
<img width="1435" height="949" alt="image" src="https://github.com/user-attachments/assets/9b8807ea-b7d7-4adf-8a36-856c69361e6b" />

Step 2: Enabling automatic security updates:
<img width="1010" height="69" alt="image" src="https://github.com/user-attachments/assets/b1928b2f-4f45-415b-bc7b-2814f8fe9df0" />

I Clicked YES when prompted:
<img width="1816" height="964" alt="image" src="https://github.com/user-attachments/assets/5103ed20-f7ce-4c73-9c58-2242c42b1e0c" />

Step 3: Verify the service is running:
<img width="1759" height="390" alt="image" src="https://github.com/user-attachments/assets/3728c4bc-48d2-491b-abc1-4eb92192832a" />

Step 4: Checking the configuration:
<img width="726" height="929" alt="image" src="https://github.com/user-attachments/assets/d94d1630-b63a-49c7-803e-770695a69063" />
<img width="747" height="945" alt="image" src="https://github.com/user-attachments/assets/e5ea6c2d-4311-408d-b43f-521e705905b8" />
<img width="935" height="868" alt="image" src="https://github.com/user-attachments/assets/6ef55e8a-3cc5-4a7d-9c8a-73ae6a4a6b9b" />

Step 5: Evidence of configuration:
<img width="940" height="165" alt="image" src="https://github.com/user-attachments/assets/c7d24158-90f4-4d88-b348-94bcdcdfc3ef" />

*Automatic Security Updates*:
<img width="1558" height="252" alt="image" src="https://github.com/user-attachments/assets/6b46954c-efd4-4fff-ba9d-cc2917198432" />

3.  Configure fail2ban for enhanced intrusion detection:

Fail2ban monitors log files (/var/log/auth.log) for suspicious activity (failed SSH logins, port scans) and automatically bans offending IP addresses by adding firewall rules.

*checking the service status after downloading it(to see active (running)), creating and editing a local configuration file, saving and restarting it*:
<img width="1822" height="709" alt="image" src="https://github.com/user-attachments/assets/49dd0adb-deaf-4df0-aacc-7c30c00299cc" />

*locating the [sshd] section and configure*:

<img width="415" height="210" alt="image" src="https://github.com/user-attachments/assets/a6adaea1-5095-4198-868f-37b5f9786982" />

4.  Security baseline verification script:

#!/bin/bash
#security-baseline.sh

echo "Security Baseline Verification Report" - Displays a header for clarity in output
date - Prints the date and time the script was executed
hostname - Displays the system hostname
hostname -I - Shows the system IP address
echo "SSH Security Configuration" - Section header for SSH configuration checks
grep "^PasswordAuthentication no" /etc/ssh/sshd_config - Verifies that password authentication is disabled
grep "^PermitRootLogin no" /etc/ssh/sshd_config - Confirms root login over SSH is disabled
grep "^PubkeyAuthentication yes" /etc/ssh/sshd_config - Confirms public key authentication is enabled
echo "Firewall Configuration" - Section header for firewall checks
ufw status - Displays firewall status and active rule
echo "Intrusion Detection (Fail2Ban)" - Section header for intrusion detection
systemctl status fail2ban - Checks whether the Fail2Ban service is running
echo "Mandatory Access Control" - Section header for MAC system
aa-status - Displays AppArmor status and enforced profiles
echo "Administrative Users" - Section header for privileged users
getent group sudo - Lists users with sudo (administrative) privileges
echo "Automatic Security Updates" - Section header for update configuration
systemctl status unattended-upgrades - Confirms automatic security updates are enabled
echo "System Resources" - Section header for system health checks
free -h - Displays memory usage
df -h / - Displays disk usage for the root filesystem
echo "Security Baseline Verification Complete" - Indicates the script has finished running

<img width="766" height="786" alt="image" src="https://github.com/user-attachments/assets/c16e2299-ecee-4dbc-a5ac-0ac9b316bebe" />

<img width="1074" height="759" alt="image" src="https://github.com/user-attachments/assets/be89f1a3-bd02-4957-97d3-f9dfd809c9b6" />

The baseline verification script confirms that essential security controls are correctly configured and active on the system.

5.  Remote monitoring script:

#!/bin/bash
#monitor-server.sh
echo "Remote Server Monitoring Report" - Prints a header for the monitoring report
date - Displays the time the monitoring check was performed
echo "Monitoring: username@server_ip" - Indicates the remote server being monitored
LOGFILE="monitoring-$(date +%Y%m%d_%H%M%S).log" - Creates a timestamped log file to store monitoring output
echo "Log file: $LOGFILE" - Displays the log file name
echo "Testing SSH connection..." - Tests SSH connectivity to the remote server
ssh username@server_ip "uptime" - Collects CPU load averages from the remote server
ssh username@server_ip "free -h" - Collects memory usage statistics from the remote server
ssh vboxuser@192.168.56.101 "_

<img width="1810" height="888" alt="image" src="https://github.com/user-attachments/assets/ba847259-9be9-476f-afed-9d7cfb74e3b9" />
<img width="1824" height="880" alt="image" src="https://github.com/user-attachments/assets/6cc24aa8-ae60-4ad0-b432-2e6b58e9d905" />
<img width="1743" height="875" alt="image" src="https://github.com/user-attachments/assets/3c92381a-ae47-403a-852d-36e36657f206" />

The monitoring script successfully collected server performance metrics remotely, demonstrating secure remote administration and monitoring capability.
