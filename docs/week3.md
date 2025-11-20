Phase 3: Application Selection for Performance Testing

This week, I shifted from security planning to preparing the test environment. To critically analyse the operating system's behaviour in Phase 6, I needed to select a suite of industry-standard tools that could generate specific, measurable workloads.

Deliverable 1: Application Selection Matrix

I selected five distinct applications to target different subsystems of the server (CPU, RAM, Disk, and Network). I chose these specific tools because they are open-source, widely used in the Linux community, and capable of running in a headless environment.


<img width="593" height="753" alt="Screenshot 2025-11-19 205632" src="https://github.com/user-attachments/assets/dd70544e-f3fc-42d6-8436-5cc54b214172" />

Deliverable 2: Installation Documentation.

All installations were done remotely through my workstation by using SSH, as per the administrative limitations of the coursework.
System Update: Initially, I checked the package repository to make sure that there were no conflicts between versions.
sudo apt update
Tool Installation: I was able to install the tools that I have chosen with the help of the apt package manager. I also installed htop to help in monitoring.
sudo apt-get install -y sysbench stress-ng iperf3 nginx htop.

<img width="655" height="218" alt="Screenshot 2025-11-18 214905" src="https://github.com/user-attachments/assets/3239c0df-8664-45eb-8732-df4c98c6d8d5" />


Successful Installation Evidence: The screenshot presented below indicates that the four major testing applications have been installed successfully and are available on the command line.

Deliverable 3: Expected Resource Profiles

Prior to the execution of the tests during Week 6, I have assumed the likely effect on the system resources:

I) Sysbench (CPU): I anticipate that the CPU utilization would run to 100 percent across all the allocated cores. System load average will increase considerably, but the memory use will be used comparatively steady.

II) Stress-ng (RAM): I expect the high Memory usage to be significantly raised. In the situation where the test uses more memory than the available physical RAM, I would imagine that I would see Swapping activity, which would probably severely affect the overall system responsiveness.

III) Sysbench (FileIO): This would mean that the CPU metrics of high I/O Wait times as the processor waits until the virtual disk has finished a read/write.

IV) Iperf3 (Network): I anticipate that network traffic (RX/TX data rates) on the enp0s3 (Host-Only) adapter will be greatly increased and possibly overwhelm the virtual link of 1Gbps.

Deliverable 4: Monitoring Strategy

Because I cannot take advantage of graphical tools such as System Monitor, I plan to measure the performance in the following CLI way in the course of the tests:

I) Real-Time Dashboard: I will open htop on another ssh window so that I can see the CPU load bars and Memory usage in real-time as the tests are being performed.

II) Disk Statistics: The iostat -x 1 command that will be used to track the percentage disk usage and read/write speed per second will be used during the FileIO tests.

III) Baseline Comparison: I will document the idle state of the server before commencing any stress test. This gives the control data required to determine the impact of the relative performance of each application.

Week 3: Reflection

In this week, I realized the need to learn the software configuration when installing it. After the installation of iperf3, the system offered me the option of whether I should start it as a daemon or not.
At first, I was going to say Yes because it would be convenient. Thoughts on the security principles of Phase 2, however, made me understand that permanent presence of a network testing tool on a port is a security risk. It creates an unwarranted point of contact. I have decided to choose No, where I just begin the service by hand only when there is a need to run some tests. This is in line with the tenet of operating the minimum necessary services in order to decrease the attack surface.

References

[1] A. Kopytov, "Sysbench Manual," GitHub, 2020. [Online]. Available: https://github.com/akopytov/sysbench. [2] C. King, "stress-ng: a tool to load and stress a computer system," Ubuntu Manpages, 2024. [Online]. Available: https://manpages.ubuntu.com/manpages/noble/man1/stress-ng.1.html. [3] ESnet, "iPerf3 User Documentation," ESnet, 2024. [Online]. Available: https://software.es.net/iperf/. [4] Nginx, "NGINX: High Performance Load Balancer, Web Server, & Reverse Proxy," Nginx.org, 2025. [Online]. Available: https://nginx.org/en/.
