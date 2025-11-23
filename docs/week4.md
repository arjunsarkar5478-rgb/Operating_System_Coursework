Phase 4: Initial System Configuration & Security Implementation

This was the week that I shifted the planning to execution. My primary goal was to deploy the underlying security measures that should be enforced under Phase 2 to convert the originally non-secure server into a hardened environment that can only be accessed through secure and cryptographic access.

All the configurations were done remotely through SSH by using my workstation, in line with the requirement of the constriction of being headless in administration.

Deliverable 3: User and Privilege Management

In accordance with the principle of least privilege, the initial critical activity that I had to perform was the creation of a new administrative user. Using default user or root as the day in day out user is a great security risk since, the attacker is left with a familiar username to attack. [1]

I added a user called host_user and put him or her in the sudo group so that when he was needed, he or she could have the administrative privileges. This I checked by logging in as the new user and ensuring that I could run root level commands.

User Creation and Privilege Assignment: The following screenshot indicates that I was able to successfully log in with the new host user account and that sudo whoami returns root, thus indicating that I can access the administrative user.


<img width="1277" height="894" alt="Screenshot 2025-11-23 105916" src="https://github.com/user-attachments/assets/2d096701-6522-4773-8fe5-a17ee167ef48" />

Deliverables 1, 4, & 5: SSH Hardening & Access


Having an account as a secured user, I concentrated on SSH service hardening. This was to be done so as to remove the password-based attacks by using key-based authentication and root logging should be banned.

Key-Based Authentication Configure

I created a strong key pair of Ed25519 SSH key on my workstation. Then I safely transferred the public key to the host code of the server via a ssh-copy-id command.[2]

I had tested the connection after deploying the key. The server was able to authenticate me based on the private key stored on my workstation and I was automatically logged into the server and no password was required.

The evidence of SSH Access (Password-less login):

<img width="1210" height="517" alt="Screenshot 2025-11-23 111019" src="https://github.com/user-attachments/assets/59ef7be4-387b-43d6-8d17-ac06b8e3ede8" />

Turning off the Root login (Configuration File Comparison)

To complete the SSH hardening, I did adjust the SSH configuration file of the server (/etc/ssh/sshd_config) so that there was an explicit disabling of direct root login. This is a very essential best practice to avoid brute-force attacks on the superuser account. [3]

Pre-Configuration Change: The default set-up permitted root to be logged in case the password authentication was disabled but the line was commented out.

Postconfiguration: I have removed the comment and changed PermitRootLogin to no.


<img width="654" height="497" alt="Screenshot 2025-11-23 131032" src="https://github.com/user-attachments/assets/468c03f2-c342-4b75-89c7-3974280f3a33" />

<img width="654" height="491" alt="Screenshot 2025-11-23 132308" src="https://github.com/user-attachments/assets/c64d1080-fb99-4c8d-8ce6-ffd3c6b55ae9" />

I then resumed the SSH service (sudo systemctl restart ssh) in order to implement such changes.


<img width="650" height="90" alt="Screenshot 2025-11-23 115815" src="https://github.com/user-attachments/assets/ed96b87c-56f8-4c0b-822f-1e004d428b83" />

Deliverables 2 & 6: Firewall Configuration

The last phase in this step was to apply the host-based firewall with the UFW (Uncomplicated Firewall). I assumed a very conservative Default Deny pose, which blocks all the incoming traffic with default and is regarded as one of the basic best practices in server security. [4]

I also took my time to configure UFW to accept incoming traffic over SSH (Port 22) before I permitted the firewall to block myself out of the remote server.

Firewall Documentation with full ruleset: The following screen shot displays the commands which are being used to configure the firewall and the eventual result of sudo ufw status, to validate the presence of the policy Default Deny (incoming) and the rule which is being used to permit SSH.


<img width="1208" height="439" alt="Screenshot 2025-11-23 125308" src="https://github.com/user-attachments/assets/63651d80-8cb8-465b-ae19-c35c8445be2e" />

Week 4: Reflection

Implementing this step was a heart thumping experience and very rewarding. A particular stress comes in when making the sshs configuration over the network; I might only need to make one error in sshd setups as well as the firewall setup and my access to the server will be denied permanently and I will have to spin up the server again. This fear strengthened the necessity of the verify then commit workflow act one important safety measure was to test the key based login in a separate terminal window and restart the service.

The application of the policy of the default deny firewall also helped me better understand how to reduce the attack surface. The default is that the server is now network transparent, excepting the one secure SSH port. This is a colossal security enhancement on the original condition in Phase 1.

References

[1] Ubuntu, "User Management," Ubuntu Server Guide, 2025. [Online]. Available: https://ubuntu.com/server/docs/security-users. [2] OpenSSH, "ssh-copy-id(1) - Linux man page," OpenSSH Manual Pages, 2024. [Online]. Available: https://man7.org/linux/man-pages/man1/ssh-copy-id.1.html. [3] ShadowSurface, "OpenSSH Security Best Practices in 2024," ShadowSurface Blog, Aug. 30, 2024. [Online]. Available: https://shadowsurface.com/blog/openssh-best-practices/. [4] Ubuntu Community, "UFW - Uncomplicated Firewall," Ubuntu Community Help Wiki, 2025. [Online]. Available: https://help.ubuntu.com/community/UFW.
