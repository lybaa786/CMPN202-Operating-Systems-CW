Phase 6: Performance Evaluation and Analysis

Testing Methodology:
For each application/service (Sysbench, Stress‑ng, fio/dd, iperf3, Nginx), I:
- Selected workloads (CPU, memory, disk I/O, network, latency, response times).
- Monitored metrics every 5 seconds for 60 seconds.
- Compared baseline, light, medium, and heavy loads.
- Identified bottlenecks.
- Applied optimisations and re‑tested.

Testing Scenarios:
- Baseline performance testing – idle system state.
- Application load testing – light, medium, heavy workloads.
- Performance analysis – bottleneck identification (CPU, memory, disk, network).
- Optimisation testing – implemented at least two improvements per subsystem, measured quantitative gains.

1. Here is my approach: a systematic process of establishing a baseline, applying load, identifying bottlenecks, optimizing performance, and then re-testing.
- Tools used: top, iostat, iperf3, ab/wrk, monitoring scripts.
- Evidence: screenshots and logs captured during each test.

2. Performance data table with structured measurements for all applications and metrics:
<img width="1269" height="438" alt="image" src="https://github.com/user-attachments/assets/57be8517-792e-4fc5-8101-2bbcc399b86b" />
<img width="1057" height="434" alt="image" src="https://github.com/user-attachments/assets/58be82e0-1d84-4cb9-9a45-7cf48f18a99c" />

4. Performance Visualisations:
- CPU vs load (Sysbench, Nginx).
- Memory vs swap usage (Stress‑ng).
- IOPS vs queue depth (fio).
- Throughput vs streams/buffer size (iperf3).
- Requests/sec vs concurrency (Nginx).

4. Testing Evidence:
Screenshots captured:
- Sysbench CPU saturation.
- Stress‑ng swap activity.
- fio random I/O performance.
- iperf3 throughput.
- Nginx load test results.

5. Network Performance Analysis:
- Throughput: 2.45–2.8 Gbps (VirtualBox limit).
- Latency: 0.10–0.15 ms.
- CPU usage: 10–28% for packet processing.
- Optimisation: Increased TCP buffer sizes → throughput +7%

6. Optimisation Analysis Results:
<img width="1025" height="429" alt="image" src="https://github.com/user-attachments/assets/349868ce-8594-4d82-b9f3-d1725238ba2b" />

<img width="1050" height="315" alt="image" src="https://github.com/user-attachments/assets/d6a8835c-ae56-4cda-b5d1-a6d70fd5cbcf" />

Critical Evaluation:
- CPU scheduling: Efficient distribution across cores (Sysbench, Nginx).
- Memory management: Swap slowed performance drastically (Stress‑ng).
- Disk I/O: Sequential fast, random slow; queue depth improved efficiency (fio).
- Network stack: Parallel streams and buffer tuning improved throughput (iperf3).
- Mixed workload: Nginx stressed CPU, memory, and network simultaneously; optimisations improved throughput and reduced bandwidth.

Conclusion:
Each workload exposed a different bottleneck. Targeted optimisations yielded measurable gains (7–40%). Correct bottleneck identification is essential—adding the wrong resource has no effect.
