# Security Incident Report

| Section 1: Identify the network protocol involved in the incident |
|-------------------------------------------------------------------|
| The network protocol involved in the incident is the HTTP protocol. This is supported by the fact that the issue was with accessing the webserver for yummyrecipesforme.com, and requests to webservers use http traffic.<br><br>Additionally, the tcpdump log shows the use of the http protocol when contacting the server.<br><br>Finally, the tcpdump log shows the malicious file is delivered to the users’ computers using the http protocol. |

<br>

| Section 2: Document the incident |
|-------------------------------------------------------------------|
| Multiple customers emailed the website’s helpdesk stating that the company website had prompted them to download a file to access free recipes. After downloading the file, customers noted that they had been redirected to a new website, and that their computer had started running more slowly.<br><br>Following the complaints, the website owner tried to log in to the admin panel but was unable to do so, indicating they had been locked out.<br><br>The security analyst created a sandbox environment to inspect the website without compromising the company network. The analyst then visited the website and ran tcpdump to collect packets created by interacting with the website. The analyst (like the customers) was prompted to download a file. They did so, and were promptly redirected to a new website (greatrecipesforme.com).<br><br>The tcpdump log shows that the browser originally requested a connection to yummyrecipesforme.com, and once this connection was established the analyst was asked to download a file. The log then shows a change in IP address, to greatrecipesforme.com, to which network traffic was then redirected.<br><br>The analyst then analysed the code for the website and file. The analyst discovered that the attacker had embedded malicious code that prompted users to download a malicious file.<br><br>As the website owner said they were unable to access their account, it is likely that the attacker used a brute force attack to gain access to the owner’s account, and subsequently changed the password.<br><br>End users’ computers were compromised as a result of the attack. |

<br>

| Section 3: Recommend potential remediations for brute force attacks |
|---------------------------------------------------------------------|
| **Disallow the use of previous passwords**<br><br> + As the attacker used a list of default passwords to gain entry to the administrative account, forbidding the use of previous/default passwords could have stopped this attack |
| **Regular password updates**<br><br> + If passwords need to be updated more frequently, even if a malicious actor is able to discover a password, there is a now a limited timeframe in which this password can be used |
| **2FA**<br><br> + Two-factor authentication requests a password and a OTP (one-time passcode) from a user. Here, even if a malicious actor is able to discover a password, they would likely not be able to access the OTP. |
