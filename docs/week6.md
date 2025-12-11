Phase 6: Performance Evaluation and Analysis  

The development of Baselines and Stress Testing Subsystems.


Introduction: In the last stage of the project, the main task I tried to accomplish was assessing the real performance features of the hardened Ubuntu Server. The aim was to provide a performance baseline at idle condition and then to test the system with stress test which was controlled by attacking the CPU, RAM and Disk I/O subsystems. This is necessary to find out how the operating system will scheduling resources will cope with the load and the possible bottlenecks that a system will have before a system enters the production phase.

Implementation and Results: I recorded the performance metrics of the system in a non-stress condition and used it as a point of reference before initiating any stress test. With monitoring tools such as htop and vmstat, I verified that the idle condition of the server was described as an extremely low activity in resource use with the CPU cores approaching zero activity and low memory usage.[3]


<img width="1920" height="1080" alt="Screenshot 2025-12-06 101325" src="https://github.com/user-attachments/assets/eb66101b-7c89-4435-a36b-b4367739d3cc" />

After making the baseline capture, I was able to perform a list of specific stress tests:

I) CPU Stress: I have been using sysbench tool to calculate prime numbers and both assigned CPU cores were loaded to capacity. The monitoring tools indicated that there was 100 percent utilization of all the cores and an increasing load average on the CPU.[2]


<img width="1920" height="1080" alt="Screenshot 2025-12-06 112129" src="https://github.com/user-attachments/assets/f4ea9ebf-6237-4ffa-8694-4b8b7bb698a6" />

II) RAM Stress: I used stress-ng to violently use a large portion of the available system memory. This put the memory management capacity of the system to the test and the memory usage bars in htop fill to the brim.[3]

<img width="1920" height="1080" alt="Screenshot 2025-12-06 114959" src="https://github.com/user-attachments/assets/3274581f-f170-48c3-ac07-09f37bb48577" />

III) Disk I/O Stress: sysbench was used in fileio mode to do very heavy random read and write to the virtual hard disk.[1]


<img width="1920" height="1080" alt="Screenshot 2025-12-06 164640" src="https://github.com/user-attachments/assets/706b33dd-2940-4c70-9344-a622e2e798e3" />

Reflections and Challenges: This concluding step was very enlightening of the dynamics of systems. The greatest conclusive was the Disk I/O test. The CPU and RAM tests had predictable behaviour, but the disk test resulted in the system load average soaring (topping at about 2.28) despite the CPU usage being not entirely busy. This was a clear indication that the virtual hard disk is a significant performance bottleneck, which occupies the CPU with a lot of time in an iowait state.

One issue that was a main challenge during this week is the failure to carry out the intended network stress test (Deliverable 6.5). Since there were network infrastructure problems that were identified as persistent in Phase 5, the server could not be reliably connected to external endpoints to execute a valid bandwidth test. Nevertheless, the passing of the baseline, CPU, RAM, and Disk I/O tests were a good foundation to studying system performance analysis.

Missing Task: Once the critical network connectivity problems had been resolved during the last phase of the project, I went back to finish the required performance evaluation activities which had been overlooked during Week 6. I wanted to compare network throughput and an attempt to test a system optimization strategy so that the evaluation was all-inclusive.

Network Performance Test

As the internet connectivity of my server was finally good, I was capable of using the scheduled network performance test to record the latency and throughput as per the brief. iperf3 utility was used to measure the speed between my workstation and the server on Host-Only network connection.

I was faced with an obstacle to security before the test could be run. Since I had hardened my UFW firewall at Phase 4 where I allowed only SSH (Port 22) it, the iperf3 connection on Port 5201 was being dropped. I had to overcome this temporarily by adding a certain UFW rule to permit traffic on that port.

Command Executed (Workstation): iperf3 -c 192.168.56.107

Result: The test successfully connected and measured an average throughput of 123 Mbits/sec.


This proved that the internal routing and the virtual network adapter were operating at its best and it was capable of the data transfer rates that was required to administer and host the applications utilized.

<img width="639" height="235" alt="Screenshot 2025-12-11 194550" src="https://github.com/user-attachments/assets/07d02bb6-a4bb-4f43-96c0-8484d3de02f1" />

Optimization Testing: Disk Scheduler

My Disk I/O stress tests in the first Phase 6 testing showed that my system Load Average spiking was very high, which meant that the disk was a bottleneck. In order to make some attempt to fix this, I applied an optimization of a change in the disk scheduler which decides the way the Linux kernel processes read/write requests.

I decided to set the default mq-deadline to none. In the case of virtualized storage, however, none can be useful because it can occasionally enhance performance since the scheduling decisions are left to the host operating system (Windows) that can be more efficient.

Prior to Optimization (Baseline): I executed the sysbench fileio test and had taken a baseline throughput whereby the test had 20,874 total events completed.

Apply Optimization: I used the command echo none sudo tee / sys/block/sda/ queue scheduler to switch the active scheduler.

Post-Optimization: I did the very same test once more. The outcome was that the test had 20,145 total events completed.

Conclusion: Performance did not increase with the optimization and, on the contrary, the disk throughput decreased slightly. This proved that, with my particular VirtualBox setup the default kernel scheduler [mq-deadline] was already more-tuned than the default none setting. This quantifiable (but negative) outcome effectively meets the need to apply and demonstrate an optimization plan.


<img width="1026" height="735" alt="Screenshot 2025-12-11 201545" src="https://github.com/user-attachments/assets/7a9a521c-ae4d-46d1-a19e-086cb574ad81" />

<img width="1037" height="739" alt="Screenshot 2025-12-11 202356" src="https://github.com/user-attachments/assets/875a8b0f-c839-4d44-867a-41ec94870099" />


References

[1] Sysbench Manual: (n.d.). Sysbench FileIO Test. [Online]. Available at: https://github.com/akopytov/sysbench#fileio-test [2] Sysbench Manual: (n.d.). Sysbench CPU Test. [Online]. Available at: https://github.com/akopytov/sysbench#cpu-test [3] Ubuntu Manpage: (n.d.). stress-ng - a tool to load and stress a computer system. [Online]. Available at: https://manpages.ubuntu.com/manpages/jammy/man1/stress-ng.1.html
