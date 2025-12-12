Phase 3: Application Selection for Performance Testing

1. Application Selection Matrix:
<img width="550" height="359" alt="image" src="https://github.com/user-attachments/assets/d797145f-999f-4fd6-babe-721076696c9b" />

Application Justifications:
•	Sysbench (CPU Workload): Sysbench is a tool that pushes the CPU to its limits so I can see how the OS handles heavy CPU work.
•	Stress-ng (Memory Workload): Stress-ng fills up RAM to test how the OS manages memory pressure, swap usage, and low-memory situations.
•	dd + fio (Disk I/O Workload): dd and fio test how fast the disk can read/write data and show how the OS handles different types of disk activity.
•	iperf3 (Network Workload): iperf3 sends large amounts of network traffic to measure bandwidth, latency, and how well the OS handles network load.
•	Nginx (Real-World Server Workload): Nginx is a real web server that lets me see how the OS manages CPU, memory, disk, and network resources at the same time.

2. Installation Documentation:
Sysbench:
<img width="1126" height="911" alt="image" src="https://github.com/user-attachments/assets/113394bf-4132-469e-97dd-9598b58f377f" />

Stress-ng:
<img width="1288" height="922" alt="image" src="https://github.com/user-attachments/assets/a7c763eb-ea0c-4576-b8ef-fd402ea0b06d" />

fio (dd is built-in):
<img width="1781" height="748" alt="image" src="https://github.com/user-attachments/assets/386e42f4-a07b-4881-983d-d2af16818b95" />
<img width="678" height="971" alt="image" src="https://github.com/user-attachments/assets/55a3ae69-a3c3-4d6c-82f0-d5d658e518a3" />

iperf3:
<img width="1196" height="692" alt="image" src="https://github.com/user-attachments/assets/4c3f0e5c-bb90-40d5-9b13-d8025bc84c95" />

Nginx:
<img width="1394" height="736" alt="image" src="https://github.com/user-attachments/assets/7d4a832b-be2d-4408-927f-93f8c9276e6e" />

4. Expected Resource Profiles:
I have created a table which documents the anticipated resource usage of each application:
<img width="1114" height="640" alt="image" src="https://github.com/user-attachments/assets/90767a29-70e8-484e-8746-f958b9ff6c03" />

4. Monitoring Strategy:
For my measurement approach I will use a combination of real-time monitoring tools and logging commands, all executed via SSH. This develops the remote monitoring skills essential for professional server administration.

