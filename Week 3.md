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





3. 
