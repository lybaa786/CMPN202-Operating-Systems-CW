Phase 4: Initial System Configuration & Security Implementation

1. Configure SSH with key-based authentication:
Benefits of this:
- No need to type passwords each time.
- Stronger security (keys are harder to brute-force).
- Enables automation (scripts, Ansible, etc.) without exposing passwords.

Step 1: Generating SSH Key Pair on my workstation using ssh-keygen:
<img width="1505" height="951" alt="image" src="https://github.com/user-attachments/assets/bc389dbe-0aef-424d-8da5-00962f55e924" />

Step 2: Copying the public key to the server:
<img width="1809" height="500" alt="image" src="https://github.com/user-attachments/assets/f8560173-ea10-48fc-9de7-2a55d097d725" />

Step 3: Testing Key-Based Authentication:
<img width="1441" height="680" alt="image" src="https://github.com/user-attachments/assets/a3d86423-5e17-4d0b-b538-951ae5b507f7" />

Step 4: Disabling Password Authentication:
<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/7a3e538f-f603-401b-83c4-e2e6f5468531" />

SSH Access Evidence showing successful connection screenshots:
I changed the passwordAuthentification to no:
<img width="962" height="107" alt="image" src="https://github.com/user-attachments/assets/d84f41ee-b9e4-484c-834d-12e7f570d86d" />
I also changed the PermitRootLogin to no and make sure the PubKeyAuthentification was set as yes:
<img width="439" height="296" alt="image" src="https://github.com/user-attachments/assets/f3abae9f-4e1c-4e7b-bac8-49fd947dd6d3" />

Step 5: Testing it on a new Terminal:


2. Configure a firewall permitting SSH from one specific workstation only:











Manage users and implement privilege management, creating a non-root administrative user:
Configuration Files with before and after comparisons

Firewall Documentation showing complete ruleset

Remote Administration Evidence demonstrating commands executed via SSH

