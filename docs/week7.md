Phase 7: Security Audit and System Evaluation

Focus

Completing a thorough security audit with industry best practices tools, fixing key blocks of infrastructure, and following remedial measures.

Introduction

In the last stage of this project, the aim of my work was to conduct a thorough security audit of the Ubuntu Server infrastructures that I had developed. I wanted to test the security state of the system with the tool Lynis auditing, carry out an external network security test with Nmap, and introduce remediation steps to increase the index of hardening my system. These duties, before I could get to them, however, had to deal with an urgent infrastructure issue of network connectivity.

Critical Infrastructure Remediation

Since the Phase 7 started, I could not install the required audit tools (lynis and nmap) as the NAT adapter on my server (which I had configured to be Adapter 2 due to the Phase 1 setup) had broken down altogether and no longer offered any internet connectivity. I also tried a lot of problem-solving such as service reset and booting up of the machine in cold but they did not help.

To continue with Phase 7, I did a significant infrastructure remediation, which was to re-configured the VirtualBox virtual hardware. I successfully changed the positions of the adapters: I Swaped Adapter 1 to a NAT and Adapter 2 to a Host-Only Adapter. After the complete re-boot of the system, my server managed to acquire new IP addresses through DHCP. I was able to regain the internet connection and re-enable secure SSH into the new Host-Only IP address, 192.168.56.107. This was a successful remediation that got the blocking problem eliminated and enabled me to install the necessary audit tools.

Security Audit Findings:

I) First System Security Scan ( Lynis): After I installed the tools, I used Lynis to inspect my system to provide an initial security score, and uncover areas of improvement.[1]

The following is a command used: sudo lynis audit system.

Initial Hardening Index: 60

<img width="1051" height="623" alt="Screenshot 2025-12-07 170502" src="https://github.com/user-attachments/assets/7b8ccb5c-4c02-42b3-a965-41d76ebcfb29" />

II) Network Security Assessment (Nmap): I have conducted an external scan of the network over my workstation terminal so as to determine the open ports and services exposed by my server on the new internal IP address.[2]

Command executed: nmap -sV 192.168.56.107

<img width="644" height="458" alt="Screenshot 2025-12-07 174049" src="https://github.com/user-attachments/assets/2fc94575-a5d9-4c3c-bf50-0028a2bddf34" />

III) SSH Security Check: I checked my SSH server configuration and ensured the security best practices that I applied in the earlier stages are still there despite the change of IP address. I made sure key-based authentication is applied, root authentication is disabled and the service is properly listening on the internal Host-Only interface only and not directly accessible to the public.

IV) Service Inventory and Justifications: My Nmap scan has found these two open TCP ports. I reviewed their necessity:

Port 22 (SSH): Justified. I need this to be able to access the headless server in a remote administered and secure way.

Port 80 (HTTP - Nginx): Justified. I discovered that this web server was installed as a dependency of other packages in the system. Although I am not actively hosting it, it does not interfere but it is secured by the firewall and can only be accessed through my internal network connection.

V) Remediation and Re-evaluation: In order to enhance the security position of my system in the light of the original systems as per the feedback provided by Lynis, I resolved to introduce system auditing. I installed and configured Linux Audit Daemon (auditd) which is used to log various events specific to the security of the system.[1]

Remediation Actions: I started the auditd service and enabled it.

After this remediation, I used a second audit of Lynis to gauge my changes.

Final Hardening Index: 61

<img width="1142" height="618" alt="Screenshot 2025-12-07 181013" src="https://github.com/user-attachments/assets/eedc7626-75b9-4b19-9722-19e3b2074642" />

With the installation of the audit daemon, I was able to raise the index of hardening of the system, which proves that my security posture has been improved.

VI) Remaining Risk Assessment: While I have hardened and audited the system, I acknowledge that risks remain. My final score of 61 indicates further room for improvement. Key risks I still need to address include untuned kernel security parameters noted in the Lynis report and the presence of the inactive Nginx service, which I should investigate for potential removal to further reduce the attack surface.

Reflections

This last stage was characterized with the need of real-life troubleshooting. Infrastructure failure changed me into a drastic restructuring of the virtual networking stack as I did not have a chance to just continue with the audit. The experience of being able to successfully reconnect myself through the replacement of roles in the case of adapters helped me to realize the necessity of adaptive problem-solving. After the completion of the next audit, the identification of the exposed services such as Nginx, and the creation of remediation to obtain a direct increase in my security score made my coursework experience a full experience.

References

[1] Lynis Documentation: (n.d.). Lynis - Security auditing tool for Linux, macOS, and UNIX-based systems. [Online]. Available at: https://cisofy.com/lynis/ [2] Nmap Reference Guide: (n.d.). Nmap Reference Guide. [Online]. Available at: https://nmap.org/book/man.html


