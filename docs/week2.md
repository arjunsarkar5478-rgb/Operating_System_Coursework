Phase 2: Security Planning and Testing Methodology

This week, I will only focus on strategy. Before I configure the server, I need a plan to secure it and a way to measure if my security changes slow it down.

Deliverable 1: Performance Testing Plan

To understand the "trade-offs" between security and performance later in the project, I need a consistent way to measure my server's health. Since I am restricted to a headless environment, I must use command-line tools over SSH.

My Remote Monitoring Methodology: I will establish a "baseline" measurement when the server is idle. Then, in Phase 6, I will run stress tests and compare the new metrics against this baseline. This will tell me if tools like the firewall or intrusion detection systems are consuming too many resources.

Metrics & Tools Selection:

I)  CPU Usage: I will use htop to monitor processor load. This is important to see if encryption (SSH) or background scanning (Fail2Ban) is slowing the system down.

II)  Memory (RAM) Usage: I will use free -m to track available memory. This is critical because if RAM fills up, the server will start "swapping" to the disk, which kills performance.

III)  Disk I/O: I will use iostat to measure read/write speeds. Log files from security tools can sometimes create a bottleneck here.

IV)  Network Latency: I will use ping from my workstation to measure packet delay. This will reveal if my strict firewall rules are adding latency to connections.

Deliverable 2: Security Configuration Checklist
I researched current 2025 best practices for Linux server security and created this checklist. I will use this to systematically harden the server in Phase 4 and Phase 5.

I) SSH Hardening (The Critical Access Point)
[ ] Disable Root Login: I will set PermitRootLogin no. Attackers always try to guess the "root" password first, so disabling direct login blocks them immediately [1], [2].

[ ] Enforce Key-Based Authentication: I will set PasswordAuthentication no. SSH keys (like Ed25519) are virtually impossible to guess, unlike passwords [2].

[ ] Idle Timeout: I will configure ClientAliveInterval to log me out if I walk away from my workstation, preventing session hijacking [2].

II) Network Security & Firewall (UFW)
[ ] Default Policies: I will set UFW to "Default Deny" (sudo ufw default deny incoming). This means the server ignores all traffic unless I specifically allow it [3].

[ ] Allow Only SSH: I will create a rule to allow traffic on Port 22 only from my workstation's IP, making the server invisible to other devices on the network [4].

III) Active Intrusion Prevention
[ ] Install Fail2Ban: I will set up Fail2Ban to read the authentication logs. If it sees repeated failed login attempts from an IP, it will update the firewall to ban that IP automatically [5].

IV) System Maintenance
[ ] Automatic Security Updates: I will configure unattended-upgrades. This ensures that if a critical vulnerability is discovered (like in the Linux kernel), my server patches itself immediately without waiting for me to log in [6].

Deliverable 3: Threat Model
To make sure my security plan actually works, I identified the three most likely attacks my server will face and planned how to stop them.

Threat 1: Brute-Force SSH Attacks
Description: This is the most common attack where bots guess thousands of passwords per second to break into the SSH port.

Mitigation Strategy: I will use a defense-in-depth approach. First, I will disable password authentication entirely (using keys only) so guessing passwords is impossible. Second, I will use Fail2Ban to detect these attacks and ban the attacker's IP address immediately [5].

Threat 2: Unpatched Software Vulnerabilities
Description: Attackers exploit bugs in old software versions to take control of the system. Since I can't monitor the news 24/7, I might miss a critical patch.

Mitigation Strategy: I will implement unattended-upgrades to automatically install security patches daily. This minimizes the "window of opportunity" for an attacker [6].

Threat 3: Unauthorized Network Reconnaissance
Description: Attackers scan the network to find open ports (like a web server or database) that I might have forgotten to close.

Mitigation Strategy: I will use the UFW firewall with a "Whitelist" strategy. By setting the default policy to deny incoming, any new service I install is automatically blocked from the network until I make a conscious decision to open it [3].

Week 2: Reflection
Planning this week really changed how I view system administration. I used to think security meant just "installing an antivirus," but I've learned it's actually about "reducing the attack surface."

For example, when researching the Threat Model, I realized that even if I have a strong password, a brute force attack can still waste my server's CPU resources just by trying to log in. That is why I decided to add fail2ban to my checklist it stops the attack at the network level, saving system resources.

I also learned about the trade-off between security and convenience. Setting up SSH keys is more work than just typing a password, and setting UFW to "Default Deny" means I have to manually open ports every time I install something new. However, this friction is necessary because it forces me to be aware of every door I open into my system.

References
[1] Security Boulevard, "10 Best Linux Server Security Practices for Sysadmin in 2024," Security Boulevard, Apr. 15, 2024. [Online]. Available: https://securityboulevard.com/2024/04/10-best-linux-server-security-practices-for-sysadmin-in-2024/. [2] ShadowSurface, "OpenSSH Security Best Practices in 2024," ShadowSurface Blog, Aug. 30, 2024. [Online]. Available: https://shadowsurface.com/blog/openssh-best-practices/. [3] Online Hash Crack, "Configure UFW Firewall 2025: Rules & Tips," OnlineHashCrack.com, 2025. [Online]. Available: https://www.onlinehashcrack.com/guides/tutorials/configure-ufw-firewall-2025-rules-tips.php. [4] Henry Will, "UFW Firewall Setup Guide for Linux," Medium, Sep. 24, 2025. [Online]. Available: https://medium.com/@henry2589will/ufw-firewall-setup-guide-for-linux-b82db2360e10. [5] Greenhost Cloud, "How To Protect SSH with Fail2Ban on Ubuntu 24.04," Greenhost.Cloud, 2025. [Online]. Available: https://greenhost.cloud/how-to-protect-ssh-with-fail2ban-on-ubuntu-24-04/. [6] Ubuntu, "Automatic updates - Ubuntu Server documentation," Ubuntu.com, 2025. [Online]. Available: https://documentation.ubuntu.com/server/how-to/software/automatic-updates/.
