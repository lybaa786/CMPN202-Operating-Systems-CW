Phase 5: Advanced Security and Monitoring Infrastructure

1. Implementing Access Control using AppArmo:
- AppArmor restricts the program behaviour and improves the ystem security.
- Each application has a profile that defines what it is allowed to do. These profiles control things like file access, permissions, and network use.
- AppArmor runs in either Enforce mode (blocks violations) or Complain mode (only logs them).

Step 1: Verifying AppArmor is Running:
Step 2: Review Active Profiles:
Step 3: Checking SSH Profile Specifically:
Step 4: Documenting the AppArmor Configuration:

2. Configuring automatic security updates:

Step 1: Installing Unattended-Upgrades:
Step 2: Configuring Automatic Security Updates:
Step 3: Enabling Automatic Updates:
Step 4: Configuring Update Frequency:
Step 5: Testing Configuration:
Step 6: Verifying Automatic Updates are Working:

3.  Configure fail2ban for enhanced intrusion detection:

Fail2ban monitors log files (/var/log/auth.log) for suspicious activity (failed SSH logins, port scans) and automatically bans offending IP addresses by adding firewall rules.

Step 1: Installing fail2ban:
Step 2: Configuring it:
Step 3: Enabling it:
Step 4: Verifying fail2ban is Monitoring SSH:
Step 5: Monitoring fail2ban Logs:

4.  Security baseline verification script:

5.  Remote monitoring script:


