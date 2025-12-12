Phase 4: Initial System Configuration & Security Implementation

1. Configure SSH with key-based authentication:
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
<img width="1811" height="793" alt="image" src="https://github.com/user-attachments/assets/1dd28db0-6493-446b-b108-d5701665aa72" />

SSH Access Evidence showing successful connection screenshots:
I changed the passwordAuthentification to no:
<img width="969" height="106" alt="image" src="https://github.com/user-attachments/assets/5cf3d2cf-95e2-4e97-9266-589f2ec0fb2d" />

I also changed the PermitRootLogin to no and made sure the PubKeyAuthentification was set as yes:
<img width="411" height="225" alt="image" src="https://github.com/user-attachments/assets/80d7ff25-f54b-401c-b9fb-4ac2fae49e75" />

Step 5: Testing it on a new Terminal:
<img width="1091" height="554" alt="image" src="https://github.com/user-attachments/assets/83367792-43f6-4612-8753-b1ba6c84fcfc" />

Remote Administration - Commands were run remotely over SSH using the vboxuser@OSCW account.

2. Configure a firewall permitting SSH from one specific workstation only:

Step 1: Checking the UFW Status:
<img width="827" height="251" alt="image" src="https://github.com/user-attachments/assets/23a96f9a-32e5-4a27-8b46-c5d5c15b17b2" />

Step 2: Setting Default Policies:
<img width="774" height="182" alt="image" src="https://github.com/user-attachments/assets/7014e118-2089-4a08-a2ff-9a8021c215d3" />

Stepn 3: Allowing SSH from Workstation IP ONLY:
<img width="1159" height="62" alt="image" src="https://github.com/user-attachments/assets/c0d3dec7-48d8-4bfa-bf0e-f36e99e092fc" />

Step  4: Enabling UFW:
<img width="1193" height="86" alt="image" src="https://github.com/user-attachments/assets/e9e61ce7-9425-40ca-b9d1-d82d37044b63" />

Step 5: Verifying Firewall Rules
<img width="958" height="288" alt="image" src="https://github.com/user-attachments/assets/d6c522c6-9096-41e1-84c9-82f6bd6e381a" />

Step 6: Testing the Firewall Rules by checking if only my IP is allowed:
<img width="834" height="235" alt="image" src="https://github.com/user-attachments/assets/08783a23-83e6-4af6-8061-4959dc148fb1" />

Configuring Files After UFW is enabled: 
<img width="1193" height="884" alt="image" src="https://github.com/user-attachments/assets/293b2368-a5a3-4886-b954-5da34e9c9b41" />

this is showing all the complex rules blocking traffic except SSH from 192.168.56.101
(it keeps going)

3. Manage users and implement privilege management, creating a non-root administrative user:
Step 1: Create New Administrative User:
<img width="1178" height="635" alt="image" src="https://github.com/user-attachments/assets/35ebcd20-6ca0-426a-9570-0e9cb05c9dcc" />

Step 2: Adding User to Sudo Group:
<img width="758" height="94" alt="image" src="https://github.com/user-attachments/assets/dfec1763-a93b-478a-b847-f6f5f75ae5ef" />

Step 3: Testing Sudo Access:
<img width="1077" height="553" alt="image" src="https://github.com/user-attachments/assets/aa484147-2f6d-498b-adff-d97b142b4531" />

Implementing privilege management:
- Using a Non-Root Account - All daily work is done as vboxuser instead of root.
- Using sudo for Admin Tasks - Administrative commands require sudo, which forces intentional privilege use and records all actions in /var/log/auth.log.
- Root Login Disabled

Finally, this shows the newly created non-root administrative user, demonstrating the implementation of privilege management:

<img width="594" height="352" alt="image" src="https://github.com/user-attachments/assets/12024c6f-974d-4205-a8b1-e8922b95d4b8" />
