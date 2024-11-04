## IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)

### Table of Contents

 <b>Building 3-Tier LAN in Packet Tracer</b>  
  - [Introduction and Initial Setup](https://github.com/GSecAwareness/LAN/blob/main/README.md)  
  - [VLANs and Layer 2 EtherChannel](https://github.com/GSecAwareness/LAN/blob/main/part2/part2.md)
  - [IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)](https://github.com/GSecAwareness/LAN/blob/main/part3/part3.md)
  - [Rapid Spanning-Tree Protocol (RSTP)](https://github.com/GSecAwareness/LAN/blob/main/part4/part4.md)  
  - [Static and Dynamic Routing (OSPF)](https://github.com/GSecAwareness/LAN/blob/main/part5/part5.md) 
  - [Network Services: DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT](https://github.com/GSecAwareness/LAN/edit/main/part6/part6.md)
---

#### In Part Three, we’ll configure IP addresses, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP). HSRP, a Cisco proprietary redundancy protocol, ensures automatic router failover to maintain high availability and prevent network outages. It assigns a virtual IP address to the router with the highest priority, making it the active router. If the active router goes offline, the next highest-priority router takes over. With a feature called preemption, we can configure the highest-priority router to automatically regain its active role once it’s back online. In this section, we’ll enable preemption to optimize our HSRP setup.

***Step 1*** *Configure the following IP addresses on R1’s interfaces and enable them:*  
    
*a. G0/0/0: DHCP client  
b. G0/1/0: DHCP client  
c. G0/0: 10.0.0.33/30  
d. G0/1: 10.0.0.37/30  
e. Loopback0: 10.0.0.76/32*

Configure the interface range g0/0/0 and g0/1/0 to get their IP addresses from DHCP. Use no shutdown to enable them because router interfaces are disabled by default. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/1%20dhcp.PNG)

Configure G0/0, which is the interface connected to CSW1. To figure out the interface relationships, use cdp or the provided “connections & ipv4 addresses” spreadsheet. Many of our interfaces are disabled by default, so show cdp neighbors doesn’t work.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/2%20int%20g0.PNG)

Configure G0/1, which is the interface connected to CSW2. Lastly, configure Loopback 0 and assign the appropriate ip address. Check your configurations with a **do show** statement. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/3%20int%20g1.PNG)

***Step 2*** *Enable IPv4 routing on all Core and Distribution switches.*  

Enter the command **ip routing** on each switch. 

***Step 3*** *Create a Layer-3 EtherChannel between CSW1 and CSW2 using a Cisco-proprietary protocol. Both switches should actively try to form an EtherChannel. Configure the following IP addresses:*    
        
*a. CSW1 PortChannel1: 10.0.0.41/30  
 b. CSW2 PortChannel1: 10.0.0.42/30*  

Configure CSW1 to be a routed port with **no switchport**. Next make the etherchannel with PAGP using desirable mode so they will actively try to form a trunk. Then configure the ip address on Port Channel.  Do the same on CSW2.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/4%20des%20mode.PNG)

From the picture we can see that Port channel 1 is RU (Layer 3 and In Use). Both of the Ports have the P flag, which is correct. Ping CSW1 to verify an active connection.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/5%20csw2%20port%20channel.PNG)

***Step 4*** *Configure the following IP addresses on CSW1. Disable all unused interfaces.*    

*a. G1/0/1: 10.0.0.34/30  
b. G1/1/1: 10.0.0.45/30  
c. G1/1/2: 10.0.0.49/30  
d. G1/1/3: 10.0.0.53/30  
e. G1/1/4: 10.0.0.57/30  
f. Loopback0: 10.0.0.77/32*  

The best way to configure these ip addresses is to write it into a notepad and paste it in. Then exit. 

**CSW1  
Interface g1/0/1  
No switchport  
ip address 10.0.0.34 255.255.255.252  
Int g1/1/1  
no switchport  
ip address 10.0.0.45 255.255.255.252  
Int g1/1/2  
no switchport   
ip address 10.0.0.49 255.255.255.252  
Int g1/1/3  
no switchport  
ip address 10.0.0.53 255.255.255.252  
Int g1/1/4  
no switchport  
ip address 10.0.0.57 255.255.255.252  
Int loopback0  
Ip add 10.0.0.77 255.255.255.255  
Int range g1/0/4-24  
shutdown**  

***Step 5*** *Configure the following IP addresses on CSW2. Disable all unused interfaces.*  

*a. G1/0/1: 10.0.0.38/30  
b. G1/1/1: 10.0.0.61/30  
c. G1/1/2: 10.0.0.65/30  
d. G1/1/3: 10.0.0.69/30  
e. G1/1/4: 10.0.0.73/30  
f. Loopback0: 10.0.0.78/32*  

**Interface g1/0/1  
No switchport  
ip address 10.0.0.38 255.255.255.252  
Int g1/1/1  
no switchport  
ip address 10.0.0.61 255.255.255.252  
Int g1/1/2  
no switchport   
ip address 10.0.0.65 255.255.255.252  
Int g1/1/3  
no switchport  
ip address 10.0.0.69 255.255.255.252  
Int g1/1/4  
no switchport  
ip address 10.0.0.73 255.255.255.252  
Int loopback0  
Ip add 10.0.0.78 255.255.255.255  
Int range g1/0/4-24  
shutdown**  

***Step 6*** *Configure the following IP addresses on DSW-A1:*    

*a. G1/1/1: 10.0.0.46/30  
b. G1/1/2: 10.0.0.62/30  
c. Loopback0: 10.0.0.79/32*    

***Step 7*** *Configure the following IP addresses on DSW-A2:* 

*a. G1/1/1: 10.0.0.50/30  
b. G1/1/2: 10.0.0.66/30  
c. Loopback0: 10.0.0.80/32*

***Step 8*** *Configure the following IP addresses on DSW-B1:*    

*a. G1/1/1: 10.0.0.54/30  
b. G1/1/2: 10.0.0.70/30  
c. Loopback0: 10.0.0.81/32*  

***Step 9*** *Configure the following IP addresses on DSW-B2:*    

*a. G1/1/1: 10.0.0.58/30  
b. G1/1/2: 10.0.0.74/30  
c. Loopback0: 10.0.0.82/32*  

Do steps 6, 7, 8, and 9 together, which involve assigning IP addresses on the distribution switches connected to the core switches.  Once again, copy and paste from a notepad for efficiency. 

The following is an example of what was copied into DSW-B2  

**interface g1/1/1  
no switchport  
ip address 10.0.0.58 255.255.255.252  
interface g1/1/2  
no switchport   
ip address 10.0.0.74 255.255.255.252  
interface loopback0  
ip address 10.0.0.82 255.255.255.255**  

***Step 10.*** *Manually configure SRV1’s IP settings:*

*a. Default Gateway: 10.5.0.1  
b. IPv4 Address: 10.5.0.4  
c. Subnet Mask: 255.255.255.0*  

Step 10 is not configured in CLI, but rather the GUI of SRV1.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/6%20GUI.PNG)

***Step 11.*** *Configure the following management IP addresses on the Access switches (interface VLAN 99), and configure the appropriate subnet’s first usable address as the default gateway.*  

*a. ASW-A1: 10.0.0.4/28  
b. ASW-A2: 10.0.0.5/28  
c. ASW-A3: 10.0.0.6/28  
d. ASW-B1: 10.0.0.20/28  
e. ASW-B2: 10.0.0.21/28  
f. ASW-B3: 10.0.0.22/28*  

Starting with ASW-A1. To configure a layer-2 switch’s default gateway, use **ip default-gateway** followed by the first usuable ip address.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/7%20default%20gateway.PNG)

Follow the same format for the remaining access switches. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/8%20ASW-A2%20default%20gateway.PNG)

In steps 12 through 19, configure HSRP on each distribution switch. In Office A, there are four subnets per switch, including Management, Phones, PCs and WiFi. In Office B, there are also four subnets including Management, Phones PCs, and the Server.  

Steps 12 through 15 concentrate on Office A. Be sure to configure HSRP version 2, as instructed. The priority of DSW-A1 should be set to 105, which is 5 above the default. Enabling preemption on DSW-A1 allows it to reclaim its role as the active router after it goes offline and comes back online. In HSRP, without preemption, if the backup router takes over, it will remain active until it goes offline, even if the original active router is restored.  

***Step 12.***  *Configure HSRPv2 group 1 for Office A’s Management subnet (VLAN 99). Make DSW-A1 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-A1.*

*a. Subnet: 10.0.0.0/28  
b. VIP: 10.0.0.1  
c. DSW-A1: 10.0.0.2  
d. DSW-A2: 10.0.0.3*  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/9%20DSW-A1%20HSRP.PNG)

Follow the same procedure on DSW-A2. There is no need to configure the priority because the default priority is set to 100, making it the backup.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/10%20DSW-A2%20HSRP.PNG)  

***Step 13.*** *Configure HSRPv2 group 2 for Office A’s PCs subnet (VLAN 10). Make DSW-A1 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-A1.*  

*a. Subnet: 10.1.0.0/24  
b. VIP: 10.1.0.1  
c. DSW-A1: 10.1.0.2  
d. DSW-A2: 10.1.0.3*  

In this part we are configuring HSRP for group 2. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/11%20DSW-A1%20HSRP%202.PNG)

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/12%20DSW-A2%20HSRP%202.PNG)

***Step 14.*** *Configure HSRPv2 group 3 for Office A’s Phones subnet (VLAN 20). Make DSW-A2 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-A2.*  

*a. Subnet: 10.2.0.0/24  
b. VIP: 10.2.0.1  
c. DSW-A1: 10.2.0.2  
d. DSW-A2: 10.2.0.3*  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/13%20DSW-A2%20group%203.PNG)

***Step 15.*** *Configure HSRPv2 group 4 for Office A’s Wi-Fi subnet (VLAN 40). Make DSW-A2 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-A2.*  

*a. Subnet: 10.6.0.0/24  
b. VIP: 10.6.0.1  
c. DSW-A1: 10.6.0.2  
d. DSW-A2: 10.6.0.3*   

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/14%20DSW-A2%20group%204.PNG)

***Step 16*** *Configure HSRPv2 group 1 for Office B’s Management subnet (VLAN 99). Make DSW-B1 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-B1.*  

*a. Subnet: 10.0.0.16/28  
b. VIP: 10.0.0.17  
c. DSW-B1: 10.0.0.18  
d. DSW-B2: 10.0.0.19*   

***Step 17*** *Configure HSRPv2 group 2 for Office B’s PCs subnet (VLAN 10). Make DSW-B1 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-B1.*  

*a. Subnet: 10.3.0.0/24
b. VIP: 10.3.0.1
c. DSW-B1: 10.3.0.2
d. DSW-B2: 10.3.0.3*  

***Step 18.*** *Configure HSRPv2 group 3 for Office B’s Phones subnet (VLAN 20). Make DSW-B2 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-B2.*  

*a. Subnet: 10.4.0.0/24  
b. VIP: 10.4.0.1  
c. DSW-B1: 10.4.0.2  
d. DSW-B2: 10.4.0.3*  

***Step 19.*** *Configure HSRPv2 group 4 for Office B’s Servers subnet (VLAN 30). Make DSW-B2 the Active router by increasing its priority to 5 above the default, and enable preemption on DSW-B2.*  

*a. Subnet: 10.5.0.0/24  
b. VIP: 10.5.0.1  
c. DSW-B1: 10.5.0.2  
d. DSW-B2: 10.5.0.3*  

Copy and paste the commands from notepad to complete these three sections in one full pass for DSW-B1. The second image will be of DSW-B2.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/15%20DSW-B1%20mega.PNG)  
![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/16%20DSW-B2%20mega.PNG)  






























