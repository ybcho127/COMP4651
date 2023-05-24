# COMP4651 Assignment 01: EC2 Measurement (5 marks)

### Deadline: 23:59, Feb 28, Tuesday

---

### Name: Young Beom CHO
### Student Id: 20467383

### Email: ybcho@connect.ust.hk

---



## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

The measurement tool used to measure CPU and memory performance is sysbench, an open-source modular, cross-platform and multi-threeaded benchmarking tool. The CPU test checks prime numbers by performing standard division of for all numbers between 2 and the square root of number defined in --cpu-max-prime, which is 40000 in this case and the remainder should be 0, higher the --cpu-max-prime, we can see the small difference easily and it is done to see the difference between t3.medium and c3.large and I see latency and event per seconds as an important metric. I have fixed the threads value to the number of the CPU of each instance(1 for t2.small and 2 for t3.medium and c3.large). The configurations will show the difference of computing power on each EC2 instances by putting stress on the CPU. I used write memory operations  with the memory total size of 20GB, in-memory with the global memory scope over 10 threads. The memory performance is observed by looking at the throughput (MB/sec).

2. (1 mark) Run your measurement tool on general purpose `t2.small`, `t3.medium`, and `c3.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **N. Virginia** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance: Latency(ms), Events per seconds (EPS)       | Memory performance|
    | ----------- | ---------------                                              | ---------------   |
    | `t2.small`  |min: 7.70 avg: 7.74 max: 7.97 95 percentile: 7.84, EPS:129.10 |  481.33 MiB/sec   |
    | `t3.medium` |min: 7.35 avg: 8.15 max: 27.67 95 percentile: 7.70, EPS:245.17|  6814.28 MiB/sec  |
    | `c3.large`  |min: 8.18 avg: 9.25 max: 9.65 95 percentile: 9.22, EPS:216.12 |  723.18 MiB/sec   |

    > Region: `N. Virginia`. Use `Ubuntu Server 20.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

For the fairness on the comparing RTT, I have transmitted the packets 10 times and recorded 4 different metrics, minumum, maximun, average, and deviation with each transmition.

    | Type                      | TCP b/w (Mbps) |  RTT (ms) min/avg/max/mdev |
    | ------------------------- | -------------- | ------- |
    | `t2.small` - ``t2.small`` |     972 Mbps   | 0.489/0.616/1.127/0.176 ms |
    | `t3.medium` - `t3.medium` |    4640 Mbps   | 0.154/0.216/0.542/0.110 ms |
    | `c3.large` - `c3.large`   |     624 Mbps   | 0.240/0.257/0.288/0.015 ms |
    | `t2.small` - `c3.large`   |     623 Mbps   | 0.443/0.501/0.796/0.099 ms |
    | `t3.medium` - `c3.large`  |     615 Mbps   | 0.564/0.604/0.748/0.054 ms |
    | `t3.medium` - `t2.small`  |     990 Mbps   | 0.658/0.744/1.054/0.113 ms |

    > Region: `N. Virginia`. Use `Ubuntu Server 20.04 LTS (HVM)` as AMI. You should launch **6** instances in total.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) |  RTT (ms) min/avg/max/mdev  |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |    25.3 Mbps   |59.734/59.800/60.029/0.089 ms|
    | N. Virginia - N. Virginia |    3440 Mbps   |0.260/0.274/0.313/0.019 ms   |
    | Oregon - Oregon           |    3030 Mbps   |0.261/0.346/0.840/0.166 ms   |

    > Region: `N. Virginia`/`Oregon`. Use `Ubuntu Server 20.04 LTS (HVM)` as AMI. All instances are `t3.medium`.
    
3. (1 mark) Is network performance consistent over time? You can do measurements at different times of a day and compare the results. Please give at least 2 possible reasons why network performance is inconsistent.

    | Time          | TCP b/w (Mbps) | RTT (ms) min/avg/max/mdev|
    | --------------| -------------- | -------- |
    |2023/2/27 5pm  |    3440 Mbps   |0.260/0.274/0.313/0.019 ms|
    |2023/2/27 12am |    3280 Mbps   |0.251/0.412/1.501/0.370 ms|
    |2023/2/28 2pm  |    3430 Mbps   |0.247/0.272/0.396/0.041 ms|
    
    > Region: `N. Virginia`. Use `Ubuntu Server 20.04 LTS (HVM)` as AMI. All instances are `t3.medium`.
    
Network performance is somewhat consistent over time, but still have some variation in t3.medium. It is because the network traffic varies by time and the change in network traffic will give different bandwidth although using same instances in same region since multiple device might use the network at the same time. Also the server load by time varies at the same time so there might be some time difference based on the server capacity.
