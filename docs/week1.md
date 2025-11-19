Phase 1: System Planning and Distribution Selection 

Here is my documentation for all the required deliverables for Phase 1: System Planning.

Deliverable 1: System Architecture Diagram

To start, I designed the system architecture required by the assessment brief. The 
goal was to build a two-system setup that separates my admin workstation from the 
server I'll be managing. This setup is a lot like a professional "jump box" or "bastion 
host" environment, which is great real-world practice [1], [2].

Deliverable 2: Server Distribution Selection Justification 

For my server VM, I decided to use Ubuntu Server 24.04.3 LTS. This was a very 
deliberate choice for a few key reasons: 

I) Long-Term Support (LTS): The "LTS" version is key. It means I get five years 
of free security updates [3], [4]. For a server, stability and security are far 
more important than brand-new features, so this felt like the most professional 
choice. 

II) Industry & Cloud Popularity: Ubuntu is the most popular Linux for cloud 
computing and servers [3], [6]. Statistics show it holds over 33% of the Linux 
market [5]. This means any skills I learn on it are directly valuable for a job, 
which hits the "Employability" theme of the module. 

III) Community & Documentation: It has a massive community, so finding 
solutions to problems is much easier. If I get stuck, someone has almost 
certainly posted a fix for it online [7]. 

IV) Ease of Use: While it's based on the super-stable Debian, Ubuntu is generally 
seen as easier to get started with [7]. Its apt package manager is also really 
simple to use for installing software.

Deliverable 3: Workstation Configuration Justification

For my workstation, I went with Option A from the brief, which is to use a separate 
Linux Desktop VM. I installed Ubuntu 25.10 Desktop for this. 

I) Mimicking Professional Practice: This setup mimics how a real sysadmin 
works. My workstation is a "bastion host" (or "jump server") [1], [2]. The server 
is on a private network, and my workstation is the only computer allowed to 
connect to it. This is a core security practice that reduces the server's attack 
surface. 

II) Enforcing the Rules: It also forces me to follow the rules of the project! By 
using a separate machine, I physically can't use the server's console. It 
guarantees I am doing all my work over SSH as required. 

III) Isolation: It keeps my project work (my scripts, my SSH keys) separate from 
my main laptop, which is just a good security habit.

Deliverable 4: Network Configuration Documentation 

I had to set up the network so my two VMs could talk to each other, but also so both 
could get to the internet for updates. 

Host-Only Network 

First, I created a "Host-Only Network" (vboxnet0) in VirtualBox. This acts as the 
private, isolated network just for my two VMs. The gateway for this network is 
192.168.56.1.

VM Network Adapter Configuration 

I configured both my Workstation and Server VMs with two network adapters each: 

I) Adapter1 (Host-Only): This connects the VM to my private vboxnet0 network. 
This is the adapter I use for SSH

II) Adapter2 (NAT): This gives the VM internet access in a safe, firewalled way. 
This will be critical later for downloading tools like fail2ban and lynis.

Deliverable 5: System Specifications (CLI Evidence) 

Here is the final proof that the whole system works. The screenshots below are taken from 
my arjun@workstation terminal, showing a successful SSH connection to my server. As required by the brief, you can see my workstation prompt arjun@workstation:
then the successful login, and then the server's prompt 
operating_system@coursework:
After logging in, I ran the 5 required commands to document the server's 
specifications (uname -a, free -h, df -h, ip addr, and lsb_release -a). 

Week 1: Reflection 

This first week was a great exercise in troubleshooting. I ran into a "Permission 
denied" error when I tried to SSH, even though I was sure I was typing the right 
password. 
My first thought was that password authentication was disabled, so I checked the 
sshd_config file, but PasswordAuthentication yes was already set correctly. 
The real solution came from checking the SSH service status on the server with 
sudo systemctl status ssh. I was able to read the logs and saw the error Failed 
password for invalid user operating_systemcoursework. This instantly showed me 
the problem: my username was actually operating_system, not 
operating_systemcoursework as I had first thought. This was a good lesson in how 
important it is to read the system logs instead of just guessing.

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

