Phase 2: Security Planning and Testing Methodology

This week, I will not think about anything but strategy. I would require a plan on how to make my server secure before I configure it and a method of measuring whether my security modifications are slowing my server.

Deliverable 1: Performance Testing Plan.

I must have a consistent way of measuring the health of my server to know the trade-offs between security and performance at a later stage in the project. I am only allowed to work in a headless environment, and therefore I am forced to use command-line tools using SSH.

My Remote Monitoring Methodology: I will provide a baseline measurement at the time when the server is not busy. Afterward, Phase 6 will involve stress tests and comparison of the new metrics to this baseline. This will inform me whether the use of tools such as the firewall or intrusion detection systems are eating up too many resources.

Metrics & Tools Selection:

I) CPU Usage: I will monitor the processor load using htop. This will be significant to determine whether the system is being slowed down by encryption (SSH) or background scanning (Fail2Ban).

II) Memory (RAM) Usage: To monitor available memory, I will monitor free -m. This is very essential in that when the RAM fills up, the server will begin swapping to the disk which will slay performance.

III) Disk I/O: iostat will be used to test the read/write speeds. Security tool log files may occasionally be a bottleneck at this stage.

IV) Network Latency: I will measure the delay of packets using ping on my work station. This will indicate whether my prohibitive firewall settings are introducing latency to traffic.

Deliverable 2: Security Configuration Checklist

I conducted a search of existing 2025 best practices in Linux server security and developed this check list. I will apply it to harden the server in Phase 4 and Phase 5 in a systematic manner.

I) SSS Hardening (The Critical Access Point) [ ] Disable Root Login: I will set PermitRootLogin no. Attackers always attempt to guess the root password at the first attempt, so by disabling direct logins, they are immediately blocked [1],[2].

[ ] Use Key-Based Authentication: I will use PasswordAuthentication no. SSH keys (such as Ed25519) are literally unguessable (compared to passwords [2]).
Idle Timeout: I will set ClientAliveInterval to expel me in case I walk in the wrong direction and leave my workstation unattended and this will prevent hijacking of my session [2].

II) Network Security/firewall (UFW) [ ] Default Policies: I shall configure UFW to default deny (sudo ufw default deny incoming). This implies that the server will not pay attention to any traffic, unless I explicitly permit it to do so [3].

[ ] Permit SSH Only: I will write a rule that only the IP of my workstation allows traffic on Port 22 and the server will be unknown to other computers on the network [4].

III) Active Intrusion Prevention [ ]Install Fail2Ban: I will install Fail2Ban to read the authentication logs. In case it notices multiple unsuccessful login attempts using an IP it will automatically change the firewall to block the IP [5].

IV) System Maintenance [ ] Automatic Security Updates: I will add unattended-upgrades. This means that in case of serious vulnerability that is discovered (such as the Linux kernel), my server automatically fixes itself without me even having to log in [6].

Deliverable 3: Threat Model

In order to ensure that my security plan is functioning correctly, I have determined the three most probable attacks my server will be subjected to and how they will be prevented.

I) Threat 1: Brute-force SSH Attacks 

Description: It is the most prevalent form of attack in which bots make guesses of thousands of passwords per second in order to crack into the SSH port.

Mitigation Strategy: I shall apply a defense in depth strategy. To begin with, I will turn off the option of password authentication altogether (only keys will be used) to make it impossible to guess the passwords. Second, I will apply Fail2Ban to identify such attacks and block the IP address of the attacker instantly [5].

II) Threat 2: Unpatched Software Vulnerabilities 

Description: Attackers use vulnerabilities in the old versions of the software to gain control of the system. As I am not able to keep an eye on the news 24/7, I may overlook a critical patch.

Mitigation Strategy: I will have unattended-upgrades which will automatically update security patches each day. This reduces the window of opportunity of an attacker [6].

III) Threat 3: Unauthorized Network Reconnaissance 

Description: The attackers scan the network and identify open ports (such as web server or database) that may be left unclosed by me.

Mitigation Strategy: I will employ the UFW firewall that has a whitelist strategy. The default policy can be configured to block the entry of any other service; therefore when any new service is added, this service is automatically blocked before I can access the network [3].

Week 2: Reflection

This kind of planning in week 1 changed my perception about system administration. I would believe that security was simply the process of installing an antivirus, but there is more than that and I only now understand that security is the process of minimizing the attack surface.
As an example, during my research into the Threat Model, I discovered that even with all the password that I have, a brute force attack can still consume my server CPU resources even before I can log in. This is why I have added fail2ban to my checklist it prevents the attack in the network level and conserves resources in the system.
I also came to know about the trade- off between security and convenience. The process of installing SSH keys is more complicated than simply typing a password, and the default configuration of UFW to "Default Deny" would ensure that I had to open ports manually each time I have installed something. This friction is however required since it makes me conscious of each door I open into my system.

References

[1] Security Boulevard, "10 Best Linux Server Security Practices for Sysadmin in 2024," Security Boulevard, Apr. 15, 2024. [Online]. Available: https://securityboulevard.com/2024/04/10-best-linux-server-security-practices-for-sysadmin-in-2024/. [2] ShadowSurface, "OpenSSH Security Best Practices in 2024," ShadowSurface Blog, Aug. 30, 2024. [Online]. Available: https://shadowsurface.com/blog/openssh-best-practices/. [3] Online Hash Crack, "Configure UFW Firewall 2025: Rules & Tips," OnlineHashCrack.com, 2025. [Online]. Available: https://www.onlinehashcrack.com/guides/tutorials/configure-ufw-firewall-2025-rules-tips.php. [4] Henry Will, "UFW Firewall Setup Guide for Linux," Medium, Sep. 24, 2025. [Online]. Available: https://medium.com/@henry2589will/ufw-firewall-setup-guide-for-linux-b82db2360e10. [5] Greenhost Cloud, "How To Protect SSH with Fail2Ban on Ubuntu 24.04," Greenhost.Cloud, 2025. [Online]. Available: https://greenhost.cloud/how-to-protect-ssh-with-fail2ban-on-ubuntu-24-04/. [6] Ubuntu, "Automatic updates - Ubuntu Server documentation," Ubuntu.com, 2025. [Online]. Available: https://documentation.ubuntu.com/server/how-to/software/automatic-updates/.
