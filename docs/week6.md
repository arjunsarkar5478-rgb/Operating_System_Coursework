Phase 6: System Performance Testing

The development of Baselines and Stress Testing Subsystems.


Introduction: In the last stage of the project, the main task I tried to accomplish was assessing the real performance features of the hardened Ubuntu Server. The aim was to provide a performance baseline at idle condition and then to test the system with stress test which was controlled by attacking the CPU, RAM and Disk I/O subsystems. This is necessary to find out how the operating system will scheduling resources will cope with the load and the possible bottlenecks that a system will have before a system enters the production phase.

Implementation and Results: I recorded the performance metrics of the system in a non-stress condition and used it as a point of reference before initiating any stress test. With monitoring tools such as htop and vmstat, I verified that the idle condition of the server was described as an extremely low activity in resource use with the CPU cores approaching zero activity and low memory usage.[3]


<img width="1920" height="1080" alt="Screenshot 2025-12-06 101325" src="https://github.com/user-attachments/assets/eb66101b-7c89-4435-a36b-b4367739d3cc" />

After making the baseline capture, I was able to perform a list of specific stress tests:

CPU Stress: I have been using sysbench tool to calculate prime numbers and both assigned CPU cores were loaded to capacity. The monitoring tools indicated that there was 100 percent utilization of all the cores and an increasing load average on the CPU.[2]


<img width="1920" height="1080" alt="Screenshot 2025-12-06 112129" src="https://github.com/user-attachments/assets/f4ea9ebf-6237-4ffa-8694-4b8b7bb698a6" />

RAM Stress: I used stress-ng to violently use a large portion of the available system memory. This put the memory management capacity of the system to the test and the memory usage bars in htop fill to the brim.[3]

<img width="1920" height="1080" alt="Screenshot 2025-12-06 114959" src="https://github.com/user-attachments/assets/3274581f-f170-48c3-ac07-09f37bb48577" />

Disk I/O Stress: sysbench was used in fileio mode to do very heavy random read and write to the virtual hard disk.[1]


<img width="1920" height="1080" alt="Screenshot 2025-12-06 164640" src="https://github.com/user-attachments/assets/706b33dd-2940-4c70-9344-a622e2e798e3" />

Reflections and Challenges: This concluding step was very enlightening of the dynamics of systems. The greatest conclusive was the Disk I/O test. The CPU and RAM tests had predictable behaviour, but the disk test resulted in the system load average soaring (topping at about 2.28) despite the CPU usage being not entirely busy. This was a clear indication that the virtual hard disk is a significant performance bottleneck, which occupies the CPU with a lot of time in an iowait state.

One issue that was a main challenge during this week is the failure to carry out the intended network stress test (Deliverable 6.5). Since there were network infrastructure problems that were identified as persistent in Phase 5, the server could not be reliably connected to external endpoints to execute a valid bandwidth test. Nevertheless, the passing of the baseline, CPU, RAM, and Disk I/O tests were a good foundation to studying system performance analysis.

References

[1] Sysbench Manual: (n.d.). Sysbench FileIO Test. [Online]. Available at: https://github.com/akopytov/sysbench#fileio-test [2] Sysbench Manual: (n.d.). Sysbench CPU Test. [Online]. Available at: https://github.com/akopytov/sysbench#cpu-test [3] Ubuntu Manpage: (n.d.). stress-ng - a tool to load and stress a computer system. [Online]. Available at: https://manpages.ubuntu.com/manpages/jammy/man1/stress-ng.1.html
