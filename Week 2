Phase 2: Security Planning and Testing Methodology

1. Performance testing plan:
Remote Monitoring Methodology:
Since my server runs headless without a graphical interface, all performance monitoring will be conducted via SSH from my workstation.
This approach develops professional server administration skills and simulates real-world cloud infrastructure monitoring.

Monitoring Approach:
Primary Monitoring Tools:
top / htop - Real-time CPU and memory usage
iostat - Disk I/O statistics
vmstat - Virtual memory and system performance
free - Memory utilization tracking
df - Disk space monitoring
netstat / ss - Network connection statistics
ping - Network latency testing
iperf3 - Network throughput measurement

Data Collection Strategy:
I will create a monitoring script (monitor-server.sh) that runs on my workstation and connects via SSH to collect performance data at regular intervals. This script will:
- Connect to the server via SSH
- Execute monitoring commands remotely
- Parse and store output data
- Generate time-series data for analysis
- Create CSV files for visualization

Testing Methodology:
Before installing any applications, I will capture a baseline performance metrics, including:
- Idle CPU usage
- Idle memory consumption
- Disk I/O at rest
- Network latency

Testing Approach:
For each application selected in Week 3, I will:

- Pre-test measurement: Record system state before starting application
- Application startup: Launch application and measure initialization impact
- Idle application: Measure resource usage when application runs but has no active users/connections
- Light load: Generate minimal activity (1-2 simulated users/connections)
- Medium load: Increase activity to moderate levels
- Heavy load: Push application to stress limits
- Post-test measurement: Record system state after stopping application

Finally, I will record these based on each test scenario:
- CPU percentage
- RAM usage
- Disk read/write speeds (MB/s)
- Disk IOPS (operations per second)
- Network throughput (Mbps)
- Network latency (milliseconds)
- Application response times

This analysis will then help me identify these factors:
- CPU-bound applications (high CPU, low I/O)
- Memory-bound applications (high RAM, potential swapping)
- I/O-bound applications (high disk wait times)
- Network-bound applications (high bandwidth, latency sensitivity)

2. Security Configuration Checklist:

SSH Hardening - secures remote access by tightening how SSH connections are made.
☐ Disable password authentication (use SSH keys)
☐ Disable root login
☐ (Optional) Change SSH port
☐ Allow SSH only from workstation IP
☐ Set idle session timeout
☐ Disable X11 forwarding

Firewall Configuration (UFW) - controls what network traffic can reach or leave the system.
☐ Enable UFW
☐ Deny all incoming traffic
☐ Allow outgoing traffic
☐ Allow SSH only from workstation IP
☐ Allow only required application ports
☐ Enable UFW logging
☐ Verify firewall rules

Mandatory Access Control (AppArmor) - restricts what programs can access, even if they’re compromised.
☐ Confirm AppArmor is enabled
☐ Review active profiles
☐ Ensure SSH daemon has an enforced profile
☐ Document current profiles
☐ Plan custom profiles for future apps

Automatic Updates - ensures security patches are installed without manual effort.
☐ Enable unattended security updates
☐ Configure security-only updates
☐ (Optional) Enable automatic reboot for kernel updates
☐ (Optional) Set email notifications
☐ Verify update logs

User Privilege Management - limits admin access to reduce the impact of compromised accounts.
☐ Create non-root admin user
☐ Add user to sudo group
☐ (Optional) Lock root password
☐ Set sudo timeout
☐ Remove unused user accounts
☐ Configure strong password policy

Network Security - adds extra protections to prevent network-based attacks.
☐ Disable IPv6 (if not needed)
☐ Enable SYN flood protection
☐ Disable ICMP redirects
☐ Enable reverse path filtering
☐ Disable source routing
☐ Apply sysctl hardening settings

3. Threat Model:
- Brute-Force SSH Attacks - where the attackers attempt to gain unauthorized access by repeatedly trying username/password combinations against the SSH service. Automated tools can try thousands of combinations per minute.
Mitigation strategies: 
- SSH Key-Based Authentication - this requires cryptographic private key for access.
- Network-Level Protection: Firewall IP Restriction - this reduces attack surface to single trusted IP.

- Privilege Escalation - where the attacker who gains limited user access exploits vulnerabilities to gain root (administrator) privileges, allowing complete system control.
Mitigation strategies:
- Principle of Least Privilege - reentering passwords after time-out using sudo commands.
- Automatic Security Updates - this fixes the kernel vulnerabilities quickly.

- Unpatched Vulnerabilities - where the attackers exploit the software vulnerabilities that exist in the operating system, kernel, and installed packages. These are exploited before they are patched.
Mitigation Strategies:
- Automatic Security Updates only - this reduces time window between vulnerability disclosure and patching.
- Minimize Installed Software - the less the packages, the smaller the attack surface. This also removes any unnecessary services.
