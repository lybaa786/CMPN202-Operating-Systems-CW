Week 1: System Planning and Distribution Selection

1. System Architecture Diagram:
<img width="441" height="341" alt="system architecture diagram" src="https://github.com/user-attachments/assets/1bd14098-2879-49d7-a992-05973435f06f" />


2. Distribution Selection Justification:
I have decided to use the Ubuntu Server 22.04 LTS as the operating system for my coursework. I have chosen this due to many reasons:
- It has a long-term support - ensures the system remains secure throughout the coursework period and beyond.
- It has an extensive community support - supports the network through problem solving and learning.
- Debian - smaller community than Ubuntu. Ubuntu's more recent packages and superior documentation better suit learning objectives.
- Fedora - less commonly used in courses and updates can sometimes break workflows.
- Arch Linux - risky for time-sensitive coursework. Ubuntu is more practical.

4. Workstation Configuration Decision:
I have decided to use my host machine as the workstation rather than creating a separate Linux Desktop VM or using a hybrid approach. This is because:
- It reduces resource consumption on my host machine.
- It is a more simplified setup and maintenence.
- There is also workflow effeciency - it provides immediate access to productivity tools without VM context-switching.
- It develops authentic professional administration workflows.

5. Network Configuration Documentation:
Adapter 1: NAT
- Purpose: Internet access for downloading updates and packages.
- How it works: The server can reach the internet, but the internet cannot reach the server.
- IP Address: Automatically assigned by VirtualBox (usually 10.0.2.15).
- Used for: Running apt update, apt upgrade, and installing software

Adapter 2: Host-Only Network
- Purpose: SSH connection from my computer to the server
- How it works: Creates a private network between my computer and the server
- My computer's IP: 192.168.56.1 (automatically assigned)
- Server's IP: 192.168.56.10 (I set this manually during installation)
- Used for: All remote administration via SSH

Using both adapters simultaneously provides:
- Internet access for legitimate purposes (updates, packages)
- Isolated management network for SSH
- Security through isolation from home network

This dual-adapter approach mirrors enterprise network designs where management networks are separated from production networks.

5. System specifications:
`uname`, `free`, `df -h`, `ip addr` and `lsb_release`:
![System Specifications] <img width="594" height="400" alt="System Specifications" src="https://github.com/user-attachments/assets/9d448c38-eee3-4f0a-9dc1-7ab9d0fb43e5" />
