#### Introduction

#### While preparing for my CCNA, I found Jeremy’s IT Lab YouTube videos incredibly helpful for the visual and interactive parts of my learning. His labs, designed for Packet Tracer, allowed students to build their skills in a network simulation program. In mid-2024, he introduced a MegaLab that brought together all the key concepts. About a week before my certification exam, I completed the lab to sharpen my CLI programming skills. I decided to revisit the lab for additional practice, this time recording my progress and sharing some insights along the way. This tutorial is part of my Office LAN project, and I’m excited to share it with you!
#### If you want to do the lab for yourself, click [here](https://www.youtube.com/watch?v=2p7-MluKAgE)
#### To get the neccessary files needed to complete the lab, click on the provided link. You will also need to install Packet Tracer. 


#### Table of Contents

- <b>Building 3-Tier LAN in Packet Tracer</b>  
  - [Introduction and Initial Setup](https://github.com/GSecAwareness/LAN/blob/main/README.md)  
  - [VLANs and Layer 2 EtherChannel](https://github.com/GSecAwareness/LAN/blob/main/part2/part2.md)
  - [IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)](https://github.com/GSecAwareness/LAN/blob/main/part3/part3.md)
  - [Rapid Spanning-Tree Protocol (RSTP)](https://github.com/GSecAwareness/LAN/blob/main/part4/part4.md)  
  - [Static and Dynamic Routing (OSPF)](https://github.com/GSecAwareness/LAN/blob/main/part5/part5.md) 




#### Part One is the initial setup of the devices and naming configuration.  

1)	Configure the hostname of the devices and save the configuration on each using the "do write" command.

  ![getcontent](https://github.com/GSecAwareness/LAN/blob/main/1%20hostname.PNG)

2)	Configure the enable secret on each router/switch. Use type 9 hashing, otherwise use type 5 hashing. The core and distribution switches are Catalyst 3650 switches, which support type 9 hashing. However, R1 and the access switches only support the type 5 hashing command.
	

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/2%20enable%20secret.PNG)

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/3%20enable%20secret%202.PNG)

3)	Configure the user account cisco with secret ccna on each router/switch. Use type 9 and type 5 hashing where applicable.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/4%20username%20password%20hash%205.PNG)

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/5%20username%20password%20hash%209.PNG)

4)	Configure the console line to require a login with a local account, set a 30-minute timeout, and enable synchronous logging. By enabling this last item, it forces syslog messages to start on the next line, instead of interrupting your input. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/6%20line%20console%20login%20with%20inactivity%20and%20logging.PNG)

5)	It is time to copy and paste the commands into the other switches. The access switches use the same commands as Router 1.

Enable secret jeremysitlab  
username cisco secret ccna  
Line console 0  
Login local  
exec-timeout 30  
logging synchronous  
do wr  
exit   

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/7%20copy%20and%20paste.PNG)

6)	Copy and paste the commands into the Core and Distribution switches. They use the following commands.

Enable algorithm-type scrypt secret jeremysitlab  
username cisco algorithm-type scrypt secret ccna  
line console 0  
login local  
exec-timeout 30  
logging synchronous  
do wr  
exit  

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/8%20copy%20and%20paste%202%20core%20and%20distribution.PNG)
