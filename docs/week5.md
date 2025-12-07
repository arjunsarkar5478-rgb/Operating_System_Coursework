Phase 5: Advanced Security and Monitoring Infrastructure 

The main goals I had during the week were to complete the security setting of the Ubuntu Server and automate my workflow. Phase 5, based on the foundations of firewall and SSH hardening of the preceding phase, was dedicated to the active intrusion prevention via Fail2Ban and Bash script writing. These scripts are scheduled to make my security baseline verification automated and capture snapshots of the system resource utilisation which will involve manual checks being replaced by automated monitoring.

Introductions and Implementation.

The information about the activities I did during Phase 5 is presented below.

I) AppArmor Configuration

I have ensured that AppArmor mandatory access control service is enabled on the server. It is already loading and implementing security profiles, which offer an extra security isolation of system services on top of the usual Linux file permissions.


<img width="643" height="261" alt="Screenshot 2025-11-29 085704" src="https://github.com/user-attachments/assets/5288a686-4f00-4ac9-b37f-b1b1402abb13" />

II) Automatic Security Updates

I ensured that the unattended-upgrades package is installed and set up. I checked the configuration files to make sure that it is configured to automatically download and install the necessary updates that are related to security so that there will be no need to manually update the server with the latest updates on a daily basis so that it is kept up to date and ready against the known vulnerabilities.

<img width="629" height="58" alt="Screenshot 2025-12-04 225522" src="https://github.com/user-attachments/assets/fbe165d8-826c-4b45-881c-7c69e3a767f9" />

III) Intrusion Prevention System (Fail2Ban)

I aimed to setup and install Fail2Ban in order to secure SSH service against brute force intrusions. This was not an easy task because there were serious network infrastructure problems (as explained in the Reflections section below). Nonetheless, I later had a chance to confirm that Fail2Ban is present, running, and I was able to create a jail.local rule, which monitors SSH connections and blocks IPs after several failures.[1][4]

<img width="1072" height="442" alt="Screenshot 2025-12-04 230046" src="https://github.com/user-attachments/assets/3d3a6905-59ec-4304-9427-76a768a3cadf" />

<img width="654" height="177" alt="Screenshot 2025-12-04 193909" src="https://github.com/user-attachments/assets/0f7eb6d0-fd6c-47ba-9bd0-f45de4bce0d1" />

IV) Security Base Line Verification Script

The security-baseline.sh Bash script was written to automate the process of checking security configurations. This script checks the UFW firewall status, SSH root login setup, AppArmor service status, and Fail2Ban SSH jail status giving a summary of PASS/FAIL.[2][3]


<img width="632" height="246" alt="Screenshot 2025-12-04 200407" src="https://github.com/user-attachments/assets/ed3f783c-5e67-4364-8ac1-7dbd78da3588" />

Text Code Image

<img width="924" height="634" alt="Screenshot 2025-12-04 231518" src="https://github.com/user-attachments/assets/d24942ab-2bcd-4dc6-b7c5-2a5e3ec9a40c" />


<img width="593" height="224" alt="Screenshot 2025-12-04 231536" src="https://github.com/user-attachments/assets/95bb671c-664f-46f9-b944-38705dfd0b2e" />

V) System Resource Monitoring Script

To give a report on the state of health of the system, I have developed a second script, system-monitor.sh. This script will give important measurement, such as disk space used in the root partition, the current memory usage, and the load averages of the CPU during the past minute, the past 5 minutes, and the past 15 minutes.[2]


<img width="608" height="342" alt="Screenshot 2025-12-04 200931" src="https://github.com/user-attachments/assets/c21024b8-c056-4ef2-b62c-7928cf60910d" />

Text Code Image


<img width="1049" height="596" alt="Screenshot 2025-12-04 231251" src="https://github.com/user-attachments/assets/716c1d22-39ad-42ef-b646-d34ba9005527" />



Reflections and Challenges

This stage was the most challenging in terms of technical aspects, which was mainly because of an ongoing and exasperating network connectivity problem with my VirtualBox setup. In the process of attempting to configure Fail2Ban, I found out that the VM of my server was not connected to the internet, although the internal Netplan setup was seeming fine--the interface was showing as "UP" and it had an IP address and default gateway was appropriate.

This took me a lot of time to troubleshoot. I tried to recreate the Netplan configuration manually on numerous occasions, made attempts to force routes by using the ip route command and made complete power cycles of the VM and my host Windows laptop. I went to the extent of updating the VirtualBox software of the host machine. The occurrence that logical settings in the Linux VM were not achieving connectivity was extraordinarily irritating and pointed to the intricacies of virtual internetting levels.

Time pressure when I was facing this blocker made me be strategic. I understood that I was not able to solve the infrastructure problem at once. I chose not to halt my whole progress and instead pivot my efforts on the scripting deliverables first. As the Bash scripts of security check and system monitoring might be written and checked locally without an internet connection, I did it as I wanted to make sure that these conditions were met.

At some point, I was in a position to confirm the installation of Fail2Ban thus enabling me to deliver on all the deliverables. This taught me that persistence is important in troubleshooting, though the necessity of maintaining a plan that would allow me to continue forward when a critical path is stalled due to reasons beyond my immediate control.

References

[1]DigitalOcean: (2024). How To Protect SSH with Fail2Ban on Ubuntu 22.04. [Online]. Available at: https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-22-04. [2]GNU Project: (2024). Bash Reference Manual. [Online]. Available at: https://www.gnu.org/software/bash/manual/bash.html [3]Canonical Ltd.: (n.d.). UFW - Community Help Wiki. [Online]. Available at: https://help.ubuntu.com/community/UFW. [4]Fail2Ban.org: (n.d.). Fail2Ban Wiki. [Online]. Available at: https://www.fail2ban.org/wiki/index.php/Main_Page
