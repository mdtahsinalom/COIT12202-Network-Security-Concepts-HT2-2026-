# WEEK-4
# SSH Hardening — Keys, fail2ban and Tunnelling
# Build and address the topology
<img width="940" height="531" alt="image" src="https://github.com/user-attachments/assets/db287000-4cc5-4913-bdb4-580fbccb79a1" />

# Successful Pinging from Admin to Server and Internal
<img width="940" height="555" alt="image" src="https://github.com/user-attachments/assets/e1c0ecea-d3ba-42e7-8906-7fa20417dae3" />

# on Admin, ssh student@10.10.1.30 logs in with no password prompt and whoami prints student
<img width="940" height="700" alt="image" src="https://github.com/user-attachments/assets/c6f98076-e416-4d80-8b4a-e2012f181a55" />

# All three results above match: one success and two Permission denied (publickey) refusals
<img width="940" height="422" alt="image" src="https://github.com/user-attachments/assets/4813b2fa-9fc4-4b8f-a072-54cf0300648a" />
<img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/b912fddf-f1af-4825-9ed2-fbfc91e67a92" />

# Fail2ban-client status sshd lists 10.10.1.20 under Banned IP list, and a further login attempt from Bastion is refused with Connection refused
<img width="940" height="871" alt="image" src="https://github.com/user-attachments/assets/7897cb92-3927-4c27-95ac-009462f2c66a" />

<img width="940" height="426" alt="image" src="https://github.com/user-attachments/assets/c8de8aaa-d0f0-47dc-a347-8e80e2ec5507" />

<img width="940" height="857" alt="image" src="https://github.com/user-attachments/assets/1362e5fd-640e-46df-bc6c-34a26a01cbb3" />

# Capture — the two .pcap files
<img width="940" height="437" alt="image" src="https://github.com/user-attachments/assets/e9737300-deb3-4682-921a-431704f4a4d2" />

<img width="940" height="328" alt="image" src="https://github.com/user-attachments/assets/fc9e303b-0465-45ed-9759-f2e373f6e9a0" />

# Question 1 — Why is Ed25519 recommended over RSA?
New SSH key pairs are recommended to use ed25519 since it's much more secure and requires significantly smaller keys than RSA. It's also efficient for key generation, authentication request signing and signature verification. In this practical, Ed25519 enabled the Admin machine to log in to the SSH server without having to send a password over the network. The practical difference is that, while RSA tends to be very large (2048 or 3072 bits) for the key, Ed25519 is fixed at a very small size. This simplifies key management while maintaining high security, making it easier for users to handle Ed25519 keys without compromising their security. RSA is still supported, especially by older systems, whereas Ed25519 is generally the preferred option if all systems are capable of supporting it.

# Question 2 — How does fail2ban defend SSH?
Fail2ban monitors the SSH server logs for multiple failed logon attempts and then takes measures to protect the server. If the number of failed login attempts for an IP address goes above the set limit, fail2ban will temporarily block this address. In this practical, maxretry = 3 was set, which means that after three unsuccessful SSH logon attempts within the set 600-seconds, the Bastion address was banned. One drawback is: fail2ban is not a substitute for good authentication. If the attacker has a valid username and private key, the attacker can still login, as there may be no repeated login failures to be caught by fail2ban. Hence, fail2ban must be used in conjunction with other measures like key authentication, restricted user accounts and disabling root logins.

# Question 3 — How do key-only authentication and fail2ban complement each other?
With key-only authentication, users can't log in with regular passwords. This significantly decreases the impact of password guessing and credential-based attacks as an attacker requires the private SSH key, not just the password. For this lab, when setting PasswordAuthentication no, the student password was not going to be used to log into the remote host via SSH. Fail2ban also adds an additional degree of security by logging frequent login logon failures and banning the IP address of the attacker for a short time. Key-only authentication keeps the authentication method secure, fail2ban keeps the number of failed connection attempts to a minimum. When combined they offer greater protection than either control alone would offer.

# Question 4 — Difference between -L and -R
Local port forward is port forwarding in which a listening port is opened on the SSH client, and the connections are routed over SSH server to the other destination. In this practical, Admin created a local port 9090 and forwarded it to 10.10.1.40:8080 via Bastion machine. So, Admin was able to access localhost:9090 via the SSH tunnel. The other direction of remote port forwarding works with ssh -R. It sets up a port on the remote SSH server and sends traffic back through the SSH tunnel to the SSH client side. Local forwarding can be helpful if a remote or internal service is required to be accessed securely by a client, and remote forwarding can be helpful if a service hosted on the client's network needs to be forwarded to the remote location.

# Question 5 — Compare the Two Packet Captures
The Admin–switch packet capture should show the communication between Admin and Bastion on SSH (port 22). The curl call to http://localhost:9090/ should not be visible in this capture, since it requires an SSH tunnel, which is encrypted. It should not be clear to the Admin that he/she is talking directly to 10.10.1.40 for this request. The Internal–switch capture should be different. After Bastion removes the SSH protection it establishes another connection to Internal on TCP port 8080, and sends the data. So the HTTP request, e.g. GET / and response that contains Internal Server can be seen in this capture. This shows that the traffic only between Admin and Bastion is protected by SSH. Once Bastion forwards the traffic to Internal, the HTTP communication is no longer protected by that SSH tunnel.


