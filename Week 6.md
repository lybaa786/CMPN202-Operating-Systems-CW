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

3. Performance Visualisations:
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


Critical Evaluation:
- CPU scheduling: Efficient distribution across cores (Sysbench, Nginx).
- Memory management: Swap slowed performance drastically (Stress‑ng).
- Disk I/O: Sequential fast, random slow; queue depth improved efficiency (fio).
- Network stack: Parallel streams and buffer tuning improved throughput (iperf3).
- Mixed workload: Nginx stressed CPU, memory, and network simultaneously; optimisations improved throughput and reduced bandwidth.

Conclusion:
Each workload exposed a different bottleneck. Targeted optimisations yielded measurable gains (7–40%). Correct bottleneck identification is essential—adding the wrong resource has no effect.
