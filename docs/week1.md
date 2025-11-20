Phase 1: System Planning and Distribution Selection 

This is my documentation of all of the deliverables that were required in the Phase 1: System Planning.

Deliverable 1: System Architecture Diagram

Firstly, I have created the system architecture that the assessment brief required. These were aimed at creating a two systems configuration that disassociates my administrator workstation and the server that I will be administering. This environment is similar to a professional jumphosting or bastion host setup which is excellent in the real world [1], [2].

<img width="1077" height="656" alt="System Architecture Diagram" src="https://github.com/user-attachments/assets/91559249-e418-4555-b59a-c28e3068db2d" />

Deliverable 2: Server Distribution Selection Justification 

In the case of my VM server, I chose to use Ubuntu Server 24.04.3 LTS. This was a highly calculated decision because of the following reasons:

I) Long-Term Support (LTS): The LTS version is important. It implies five years of complimentary security updates [3], [4]. To a server, stability and security are much more significant than new features, and as such, this was the most professional decision.

II) Industry, Cloud Popularity: Ubuntu is most popularly used in cloud computing and server [3], [6]. According to the statistics, it controls more than 33 percent of the Linux market [5]. This implies that whatever skills I acquire on it are directly marketable in a job, which strikes the theme of the module on Employability.

III) Community and Documentation: It possesses a huge community, thus it is much easier to find solutions to issues. In case I become stuck, there will be a solution posted somewhere online almost definitely [7].

IV) Accessibility: Ubuntu is considered to be easier to start with, although it is also founded on the rock-solid Debian [7]. Its package management system is also most efficient and very easy to use when installing software.

Deliverable 3: Workstation Configuration Justification

In the case of my work station, I have chosen Option A of the brief which consists of using a different Linux Desktop VM. I installed Ubuntu version 25.10 Desktop because of this.

I) Simulation of Professional Practice: This configuration is a replica of real-world activity of a sysadmin. My workstation is a jump server (or bathtion host) [1], [2]. The server is within a private network and my workstation is the only computer that is authorized to be connected to it. It is one of the fundamental security practices that decrease the attack surface of the server.

II) Applying the Rules: It also makes me abide by the regulations of the project! I can not physically use the console of the server, by using a separate machine. It has ensured that I am performing all my tasks as needed over SSH.

III) Isolation: It maintains my project work (my scripts, my SSH keys) separated out of my main laptop, which is only a good security behavior.

Deliverable 4: Network Configuration Documentation 

I needed to configure the network, such that my two VMs could communicate with each other, as well as both could access the internet to update.

Host-Only Network 

The first thing I did was to create a host-only network (vboxnet0) in VirtualBox. This is the personal, closed network only of my two VMs. The IP address of the gateway to this network is 192.168.56.1.

<img width="1047" height="743" alt="Screenshot 2025-11-12 212050" src="https://github.com/user-attachments/assets/b9d40c81-7d63-4b89-a75d-eb469849bbdc" />

VM Network Adapter Configuration 

My Workstation and Server VMs were set up with two network adapters each:

I) Adapter1 (Host-Only): This is an interface that connects the VM to my own vboxnet0 network. This is my ssh adapter.

<img width="952" height="599" alt="Screenshot 2025-11-12 212731" src="https://github.com/user-attachments/assets/8d27ad7b-2723-4dec-9a35-0823ad79a8d6" />
<img width="950" height="597" alt="Screenshot 2025-11-12 212744" src="https://github.com/user-attachments/assets/aa2ec07e-9b47-47ec-97f8-d58e8e843354" />


II) Adapter2 (NAT): This provides the VM with access to the internet in a safe and firewalled manner. This will come in handy in future to download applications such as fail2ban and lynis.

<img width="957" height="592" alt="Screenshot 2025-11-13 101933" src="https://github.com/user-attachments/assets/40e647ae-bc99-4437-8558-03d6830c8a01" />
<img width="962" height="595" alt="Screenshot 2025-11-13 101952" src="https://github.com/user-attachments/assets/7e01a5e1-dcbd-4cc7-acc5-f9db8a8bc0e6" />


Deliverable 5: System Specifications (CLI Evidence) 

This is the last evidence that the entire system works. The figures below are captured on my terminal arjun@workstation and it reflects successful SSH connection to my server.

<img width="1279" height="888" alt="Screenshot 2025-11-12 220448" src="https://github.com/user-attachments/assets/0743cee9-1122-404e-b5aa-5716592ebc40" />
<img width="1190" height="705" alt="Screenshot 2025-11-13 112032" src="https://github.com/user-attachments/assets/82f0e6b2-b0e5-4603-8784-4dc80511b404" />
<img width="1193" height="405" alt="Screenshot 2025-11-13 112112" src="https://github.com/user-attachments/assets/f35f2009-5336-485b-b943-fe6bde86485b" />





As the brief asked, you can see that after I logged in I entered my workstations prompt arjun@workstation: and then the successful log, and the prompt on the server operatingsystem@coursework: and then I proceeded to run the 5 commands that were required to capture the specifications of the server (uname -a, free -h, df -h, ip addr, and lsbrelease -a).

Week 1: Reflection 

The first week was an excellent ordeal in troubleshooting. I encountered a Permission denied error when I made an attempt to SSH, although I was certain that I was entering the correct password. I assumed that password authentication was turned off, so I went and checked the sshdconfig file, but PasswordAuthentication yes was already set appropriately. The actual solution was a search of the SSH service status on the server using sudo systemctl status ssh. I could read the logs and observed the error Failed password of invalid user operatingsystemcoursework. This immediately revealed me the issue: I was in fact called operatingsystem and not operatingsystemcoursework. That was a lesson, on how to be able to read the system logs rather than just make an educated guess.

References 

[1] JumpCloud, "What is a Jump Server / Bastion Host?," JumpCloud.com, 2025. 
[Online]. Available: https://jumpcloud.com/it-index/what-is-a-jump-server-bastion
host. [2] H. Security, "What Is a Jump Server? Definition and Safety Measures," 
Heimdal Security Blog, Oct. 15, 2025. [Online]. Available: 
https://heimdalsecurity.com/blog/what-is-a-jump-server/. [3] Canonical, "Ubuntu 
Server - for scale out workloads," Ubuntu.com, 2025. [Online]. Available: 
https://ubuntu.com/server. [4] T. K. Fasthosts, "CentOS vs Debian vs Ubuntu: Which 
is the best Linux distribution?," Fasthosts Blog, Apr. 15, 2020. [Online]. Available: 
https://www.fasthosts.co.uk/blog/linux-distribution-comparison/. [5] "Linux Statistics 
2024," Enterprise Apps Today, 2024. [Online]. Available: 
https://www.enterpriseappstoday.com/stats/linux-statistics.html. [6] RunCloud, "16 
Best Linux Distros in 2025," RunCloud Blog, Oct. 21, 2025. [Online]. Available: 
https://runcloud.io/blog/best-linux-distros. [7] MangoHost, "Ubuntu Server vs. Other 
Operating Systems: Why It's the Best Choice," MangoHost Blog, May 17, 2023. 
[Online]. Available: https://mangohost.net/blog/ubuntu-vs-other-operating-systems
better-choice-for-a-server/.
Footer

