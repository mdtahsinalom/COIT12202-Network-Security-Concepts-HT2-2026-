# Week-3
## Password Hashing and Storage 

# Network topology
<img width="940" height="472" alt="image" src="https://github.com/user-attachments/assets/1e4d70d0-c395-47c9-9d78-fd396a760da5" />

# Showing the three algorithms
<img width="940" height="380" alt="image" src="https://github.com/user-attachments/assets/cd2f6a59-10e2-4c6a-930a-2bde5c6ad3c1" />

# Password policy testing — recommended evidence
<img width="940" height="760" alt="image" src="https://github.com/user-attachments/assets/46b8a1ad-5750-4714-b0c5-b3c5641441ca" />

# Account lockout — recommended evidence
<img width="940" height="760" alt="image" src="https://github.com/user-attachments/assets/54033eb4-aa04-4239-a949-d68ae3d2d8f0" />


# Cracking result
<img width="940" height="475" alt="image" src="https://github.com/user-attachments/assets/f456e28f-6d8f-4ce8-a2f9-9a3a797aca4a" />

# Question 1: What do the prefixes $1$, $6$, and $y$ in /etc/shadow indicate, and why does the algorithm choice matter for password security?
The prefixes identify which password hashing algorithm Linux used to store the password:
$1$ = MD5-crypt
$6$ = SHA-512-crypt
$y$ = yescrypt
The algorithm is important because the different algorithms are going to be different strengths in terms of how they resist password attacks. MD5-crypt is a relatively quick hash function, so that it can test out guesses of passwords in a timely manner. SHA-512-crypt requires more computing power, yescrypt is deliberately harder to guess passwords with. Thus, less powerful password hashing functions tend to make offline cracking more difficult, if done on a large scale. The definitions of the prefixes are provided directly in the lab.

# Question 2: Why is a slow hashing algorithm such as yescrypt or bcrypt preferred for password storage even though it uses more CPU during legitimate logins?

A slow password-hashing algorithm is preferred since everyone using the system is impacted differently from anyone using it evil. A legitimate user is likely to only have to have the server calculate the hash once during login, and thus it's okay if there's a slight delay. If an attacker is trying to attack the password in an offline mode, he or she might have to try millions or even billions of passwords. The more time and resources it takes to try each of the possibilities, the more expensive the attack becomes. Hence, an attacker won't be able to do as many password guesses in a certain amount of time when the passwords are in a yescrypt system.

# Question 3: What is a salt in password hashing, and what attack does it prevent? Identify the salt in one of your /etc/shadow entries.
Salt is some random value appended to a password prior to calculating the hash of the password. It's designed to prevent users from having the same password from generating the same hash. Salting also helps to make it difficult for attackers to use precalculated hash tables, including rainbow tables, for large collections of password hashes. If my MD5 was, say: user_md5:$1$abc12345$xxxxxxxxxxxxxxxxxxxxxx:... then: $1$ identifies MD5-crypt and: abc12345 is the salt. If not then do not use abc12345 in your answer. Run: grep '^user_md5:' /etc/shadow and identify your own salt. The algorithm prefix is followed by the salt, and it is preceded by the next $, according to the lab.

# Question 4: What is a potential downside of pam_faillock, and how might an attacker exploit it?
One downside of using account lockout policies is that they can be a denial-of-service threat. For instance, if an account is locked after five wrong logins, an intruder doesn't have to know the correct password. The attacker might take the time to enter the wrong password 5 times into a victim's account. This will now "lock" out the legitimate user's account for the set time, in this lab it is: 300 seconds An attacker could continue to do so and make it harder for the legitimate user to access the system. So, while account lockout policies can help minimize brute force login attempts, they need to be carefully designed to minimize the likelihood of successful account lockout attacks.
