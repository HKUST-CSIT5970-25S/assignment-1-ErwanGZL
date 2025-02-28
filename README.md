[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Gonzales Erwan Theo
### Student Id: 21134672
### Email: etgonzales@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

> **The measurement tool chosen for this task is `sysbench`**
> 
> For the CPU performance benchmark, we are using the following command:
> `sysbench cpu --cpu-max-prime=200000 --threads=8 --time=300 run`
> - define prime search objective to 200000
> - use multiple threads to ensure realistic usage
> - time limit of 5 minutes
> This command allow for benchmarking a realistic workload using multiple thread over a long period of time
>
> For the Memory performance benchmark, we are using the following command:
> `sysbench memory --memory-block-size=1M --memory-total-size=100G --memory-oper=write --memory-access-mode=rnd --threads=8 run`
> - execute memory write operations
> - block size of  1mb
> - limit set  to 100gb
> - access at random
> - leveraging multithreading
> 
> Using those parameters we can command a realistic memory workload
>
> As for CPU results we can take the number of `event per second` as an indicator of performance and see performance increase with each tier of machine.
> For Memory results, we can look at the number of `MiB transferred` which is also our total number of operation. Again performance results seem to enhance with machine tier. Although the `c5d.large` is falling behind the `t2` units.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

| Size        | CPU performance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Memory performance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `t2.micro`  | CPU speed:<br>    **events per second:    37.42**<br><br>General statistics:<br>    total time:                          300.0931s<br>    total number of events:              11229<br><br>Latency (ms):<br>         min:                                  116.80<br>         avg:                                  213.73<br>         max:                                  273.00<br>         95th percentile:                      240.02<br>         sum:                              2399954.16<br><br>Threads fairness:<br>    events (avg/stddev):           1403.6250/2.69<br>    execution time (avg/stddev):   299.9943/0.03 | Total operations: 17955 ( 1794.54 per second)<br><br>**17955.00 MiB transferred (1794.54 MiB/sec)**<br><br><br>General statistics:<br>    total time:                          10.0034s<br>    total number of events:              17955<br><br>Latency (ms):<br>         min:                                    0.42<br>         avg:                                    4.44<br>         max:                                   31.56<br>         95th percentile:                       12.52<br>         sum:                                79674.12<br><br>Threads fairness:<br>    events (avg/stddev):           2244.3750/11.25<br>    execution time (avg/stddev):   9.9593/0.02 |
| `t2.medium` | CPU speed:<br>    **events per second:    69.67**<br><br>General statistics:<br>    total time:                          300.0565s<br>    total number of events:              20905<br><br>Latency (ms):<br>         min:                                   26.42<br>         avg:                                  114.80<br>         max:                                  210.06<br>         95th percentile:                      161.51<br>         sum:                              2399900.90<br><br>Threads fairness:<br>    events (avg/stddev):           2613.1250/2.26<br>    execution time (avg/stddev):   299.9876/0.02 | Total operations: 30367 ( 3035.63 per second)<br><br>**30367.00 MiB transferred (3035.63 MiB/sec)**<br><br><br>General statistics:<br>    total time:                          10.0020s<br>    total number of events:              30367<br><br>Latency (ms):<br>         min:                                    0.42<br>         avg:                                    2.63<br>         max:                                   77.13<br>         95th percentile:                        7.70<br>         sum:                                79748.44<br><br>Threads fairness:<br>    events (avg/stddev):           3795.8750/27.15<br>    execution time (avg/stddev):   9.9686/0.01 |
| `c5d.large` | CPU speed:<br>    events per second:    29.60<br><br>General statistics:<br>    total time:                          300.1487s<br>    total number of events:              8884<br><br>Latency (ms):<br>         min:                                   85.12<br>         avg:                                  270.23<br>         max:                                  410.02<br>         95th percentile:                      272.27<br>         sum:                              2400724.12<br><br>Threads fairness:<br>    events (avg/stddev):           1110.5000/1.32<br>    execution time (avg/stddev):   300.0905/0.05      | Total operations: 28600 ( 2859.09 per second)<br><br>28600.00 MiB transferred (2859.09 MiB/sec)<br><br><br>General statistics:<br>    total time:                          10.0017s<br>    total number of events:              28600<br><br>Latency (ms):<br>         min:                                    0.64<br>         avg:                                    2.79<br>         max:                                   12.70<br>         95th percentile:                        6.67<br>         sum:                                79908.96<br><br>Threads fairness:<br>    events (avg/stddev):           3575.0000/8.09<br>    execution time (avg/stddev):   9.9886/0.01      |

> Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

| Type                      | TCP b/w (Mbps) | RTT (ms) |
| ------------------------- | -------------- | -------- |
| `t3.medium` - `t3.medium` | 3490           | 0.359    |
| `m5.large` - `m5.large`   | 4920           | 0.151    |
| `c5n.large` - `c5n.large` | 4950           | 0.166    |
| `t3.medium` - `c5n.large` | 2380           | 0.647    |
| `m5.large` - `c5n.large`  | 4960           | 0.198    |
| `m5.large` - `t3.medium`  | 2320           | 0.751    |

> Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

1. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

| Connection                | TCP b/w (Mbps) | RTT (ms) |
| ------------------------- | -------------- | -------- |
| N. Virginia - Oregon      | 32.9           | 64.603   |
| N. Virginia - N. Virginia | 3880           | 0.299    |
| Oregon - Oregon           | 4700           | 0.196    |


> Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
