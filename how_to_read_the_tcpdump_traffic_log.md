# How to read the tcpdump traffic log

This reading explains how to identify the brute force attack using tcpdump.

![img_1](https://github.com/andrewrodgers90/apply_OS_hardening_techniques/assets/132149730/f30dd928-b36b-40db-9798-e19e01ce6728)

The first section of the DNS & HTTP traffic log file shows the source computer (**your.machine.52444**) using port **52444** to send a DNS resolution request to the DNS server (**dns.google.domain**) for the destination URL (**yummyrecipesforme.com**). Then the reply comes back from the DNS server to the source computer with the IP address of the destination URL (**203.0.113.22**). 

![img_2](https://github.com/andrewrodgers90/apply_OS_hardening_techniques/assets/132149730/85262a0d-d7af-42e9-91cb-07b9bfddb8a8)

The next section shows the source computer sending a connection request (**Flags [S]**) from the source computer (**your.machine.36086**) using port **36086** directly to the destination (**yummyrecipesforme.com.http**). The **.http** suffix is the port number; **http** is commonly associated with port 80. The reply shows the destination acknowledging it received the connection request (**Flags [S.]**). The communication between the source and the intended destination continues for about 2 minutes, according to the timestamps between this block (**14:18**) and the next DNS resolution request (see below for the **14:20** timestamp). 

TCP Flag codes include:


**Flags [S]**  - Connection **S**tart
<br>
**Flags [F]**  - Connection **F**inish 
<br>
**Flags [P]**  - Data **P**ush
<br>
**Flags [R]**  - Connection **R**eset
<br>
**Flags [.]**  - Acknowledgment

![img_3](https://github.com/andrewrodgers90/apply_OS_hardening_techniques/assets/132149730/4ae46073-2bcf-44a3-a631-44044f2142ed)

The log entry with the code **HTTP: GET / HTTP/1.1** shows the browser is requesting data from **yummyrecipesforme.com** with the **HTTP: GET** method using **HTTP** protocol version **1.1**. This could be the download request for the malicious file. 

![img_4](https://github.com/andrewrodgers90/apply_OS_hardening_techniques/assets/132149730/94f2009f-8cea-47f7-9493-b5025bceeece)

Then, a sudden change happens in the logs. The traffic is routed from the source computer to the DNS server again using port **.52444** (**your.machine.52444 > dns.google.domain**) to make another DNS resolution request. This time, the DNS server routes the traffic to a new IP address (**192.0.2.172**) and its associated URL (**greatrecipesforme.com.http**). The traffic changes to a route between the source computer and the spoofed website (outgoing traffic: **IP your.machine.56378 > greatrecipesforme.com.http** and incoming traffic: **greatrecipesforme.com.http > IP your.machine.56378**). Note that the port number (**.56378**) on the source computer has changed again when redirected to a new website.
