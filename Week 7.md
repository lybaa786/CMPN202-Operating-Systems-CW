Phase 7: Security Audit and System Evaluation

Part 1: Initial Security Assessment with Lynis

1.1: Installing and Running Lynis:

*Running comprehensive security audit*:
<img width="916" height="896" alt="image" src="https://github.com/user-attachments/assets/aeffe0d3-f4d9-4a50-a425-0fe7acd6c12b" />
<img width="1081" height="866" alt="image" src="https://github.com/user-attachments/assets/3ec210dc-a357-45fc-8801-aec432d928c8" />
....
<img width="1411" height="920" alt="image" src="https://github.com/user-attachments/assets/5da1021a-f37d-4da8-b346-5b6e9cb82c9e" />

This shows Lynis audit in progress.

1.2: Analysing Lynis Results:

*Extracting Hardening index*:
<img width="1401" height="81" alt="image" src="https://github.com/user-attachments/assets/126cec58-4914-405b-8cf3-c95e2faa9341" />

*Listing all warnings*:
<img width="1721" height="173" alt="image" src="https://github.com/user-attachments/assets/0070e99e-f09e-47f3-84bd-4478739ec778" />

*Listing all suggestions*:
<img width="1831" height="888" alt="image" src="https://github.com/user-attachments/assets/ccc9ff82-38ff-4d12-abd4-abf40fb6b11e" />
<img width="1820" height="939" alt="image" src="https://github.com/user-attachments/assets/a40a194d-bb15-4ecc-b716-c671519e3fd9" />

*Viewing specific test categories*:
<img width="1709" height="823" alt="image" src="https://github.com/user-attachments/assets/136b8c39-10ef-49ad-b74a-8d4adf7c23c5" />

Audit summary table:
<img width="823" height="587" alt="image" src="https://github.com/user-attachments/assets/f89723c5-8948-4334-a722-bcd7c83c2bf2" />

10 Most Critical Lynis Warnings:

- SSH root login enabled: This allows direct login as the root user over SSH. If compromised, an attacker gains full system control immediately. Best practice is to disable root login and use sudo.
- Firewall not enabled or not properly configured: Without an active firewall, all open services may be accessible from the network, increasing exposure to external attacks.
- Weak SSH configuration settings detected: Default SSH settings may allow insecure ciphers, protocols, or unlimited login attempts, making brute-force attacks more likely.
- Automatic security updates not enabled: Security patches are not applied automatically, leaving the system vulnerable to known exploits until manual updates are performed.
- Outdated or unnecessary services running: Services that are not required increase the system’s attack surface and may contain exploitable vulnerabilities.
- Insecure file permissions on system files: Files with overly permissive access rights can allow unauthorised users to read or modify sensitive data.
- Audit logging not fully configured: Incomplete logging reduces the ability to detect, investigate, and respond to security incidents.
- Log rotation not configured: Log files may grow indefinitely, potentially filling disk space and preventing important system services from functioning correctly.
- Kernel hardening parameters not optimised (sysctl): Missing kernel security settings reduce protection against network-based and privilege escalation attacks.
- Password policy not sufficiently enforced: Weak password complexity or ageing policies increase the risk of credential compromise through guessing or brute-force attacks.

1.3: Prioritising Security Improvements:
<img width="1147" height="735" alt="image" src="https://github.com/user-attachments/assets/dfe6523a-d030-42d5-8423-88d51a0f33c3" />

Part 2: Network Security Assessment:

2.1: Port Scanning with nmap:

*Verifying nmap version*:
<img width="1810" height="302" alt="image" src="https://github.com/user-attachments/assets/01aa4d5c-a274-4f05-b4ff-343fc7843d5f" />

*Basic port scan and service version (sV) detection scan*:
<img width="1290" height="405" alt="image" src="https://github.com/user-attachments/assets/ac257eea-3d48-4cd3-a668-123adf3e454f" />

*scanning common parts and ALL parts and saving it to file*:
<img width="1242" height="406" alt="image" src="https://github.com/user-attachments/assets/385197c6-16b7-4b27-a93d-5dd306ee7e35" />

*attempting OS detection*:
<img width="1239" height="201" alt="image" src="https://github.com/user-attachments/assets/db234719-58ec-4c48-9be0-16e456c94236" />

Port Inventory Table:
<img width="1151" height="434" alt="image" src="https://github.com/user-attachments/assets/98418385-3d8a-46fa-9800-a4218ddf37fe" />

2.2: Verifying Firewall Effectiveness:

*reviewing complete firewall configuration*:
<img width="979" height="535" alt="image" src="https://github.com/user-attachments/assets/9f21392b-0fc9-4d91-bd51-e75e001ff290" />

*verifying default policies*:
<img width="1649" height="860" alt="image" src="https://github.com/user-attachments/assets/bcc88cb4-21c8-4a9c-996b-994ba655bde5" />
....

*checking iptables rules directly*:
<img width="1497" height="838" alt="image" src="https://github.com/user-attachments/assets/fbec2702-d0d5-4dd8-980d-78fda6a4d9f0" />
....

*documenting firewall configuration and testing firewall from workstation by attempting connections to blocked ports*:
<img width="1307" height="189" alt="image" src="https://github.com/user-attachments/assets/da344955-dde3-45c8-8c9f-b432105b773e" />

Firewall Ruleset Documentation:
<img width="1478" height="560" alt="image" src="https://github.com/user-attachments/assets/840d5375-1a6c-4367-9109-b4ffde8d3c4e" />

Firewall Effectiveness Testing:
<img width="1455" height="250" alt="image" src="https://github.com/user-attachments/assets/5dcd8774-a1c2-4f3d-9de0-8eed3807a366" />

2.3: Analysing Running Services:

*listing all listening services:*
<img width="1515" height="406" alt="image" src="https://github.com/user-attachments/assets/4dd6b653-d930-4aa9-b261-4298973042bc" />

*identifying services by process:*:
<img width="1228" height="424" alt="image" src="https://github.com/user-attachments/assets/97b1ca7b-9a34-477e-9abe-3d3960ebbe42" />

*listing all running services*:
<img width="1581" height="818" alt="image" src="https://github.com/user-attachments/assets/427c410c-1277-4002-a33e-0f69478a82cc" />




Lynis scores before and after remediation:

Network security testing results:

SSH security verification:

Service inventory with justifications:

Remaining risk assessment:

