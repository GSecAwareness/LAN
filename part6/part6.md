## Network Services: DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT  

### Table of Contents

 <b>Building 3-Tier LAN in Packet Tracer</b>  
  - [Introduction and Initial Setup](https://github.com/GSecAwareness/LAN/blob/main/README.md)  
  - [VLANs and Layer 2 EtherChannel](https://github.com/GSecAwareness/LAN/blob/main/part2/part2.md)
  - [IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)](https://github.com/GSecAwareness/LAN/blob/main/part3/part3.md)
  - [Rapid Spanning-Tree Protocol (RSTP)](https://github.com/GSecAwareness/LAN/blob/main/part4/part4.md)  
  - [Static and Dynamic Routing (OSPF)](https://github.com/GSecAwareness/LAN/blob/main/part5/part5.md) 
  - [Network Services: DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT](https://github.com/GSecAwareness/LAN/edit/main/part6/part6.md)
---

#### In this section, we’ll configure key networking services to manage and secure network operations. DHCP will automatically assign IP addresses to devices, simplifying network management. DNS will resolve domain names to IP addresses, enabling easy access to resources by name. NTP will synchronize device clocks, ensuring consistent timekeeping across the network. SNMP will monitor device performance and alert administrators to potential issues. Syslog will centralize logs, helping with monitoring and troubleshooting. FTP will enable secure file transfers, and SSH will allow encrypted remote management of devices. Finally, NAT will map private IP addresses to a public one, securing and optimizing internet access. This will be a big section, so lets dive in. 

***Step 1*** *Configure the following DHCP pools on R1 to make it serve as the DHCP server for hosts in Offices A and B. Exclude the first ten usable host addresses of each pool; they must not be leased to DHCP clients.*  
*a. Pool: A-Mgmt  
   i. Subnet: 10.0.0.0/28  
   ii. Default gateway: 10.0.0.1  
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)  
   v. WLC: 10.0.0.7  
b. Pool: A-PC  
   i. Subnet: 10.1.0.0/24  
   ii. Default gateway: 10.1.0.1  
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)  
c. Pool: A-Phone  
   i. Subnet: 10.2.0.0/24  
   ii. Default gateway: 10.2.0.1  
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)  
d. Pool: B-Mgmt  
   i. Subnet: 10.0.0.16/28  
   ii. Default gateway: 10.0.0.17  
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)  
   v. WLC: 10.0.0.7   
e. Pool: B-PC  
   i. Subnet: 10.3.0.0/24  
   ii. Default gateway: 10.3.0.1  
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)  
f. Pool: B-Phone  
   i. Subnet: 10.4.0.0/24  
   ii. Default gateway: 10.4.0.1   
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)  
g. Pool: Wi-Fi  
   i. Subnet: 10.6.0.0/24  
   ii. Default gateway: 10.6.0.1  
   iii. Domain name: jeremysitlab.com  
   iv. DNS server: 10.5.0.4 (SRV1)*    

Go into the CLI of R1 and enter the excluded addresses first. Next, configure the dhcp pool A-Mgmt and set the pool range based on the instructions 10.0.0.0/28. Set the default router as 10.0.0.1 and the domain name as jeremysitlab.com. Configure the DNS server 10.5.0.4 and the WLC address for lightweight access points. To specify a WLC address, use the option keyword, using option code 43 and then enter the ip address.   
Option 43 is commonly used in wireless networks where Lightweight Access Points (LAP) require the IP address of a Wireless LAN Controller (WLC). With option 43, the DHCP server can pass controller information to access points.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part6/1%20dhcp.PNG)  
   
For the remaining parts of Step 1, follow the same method as before. I wrote everything in a notepad to check for accuracy. Once I was confident the commands were correct, I pasted them in.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part6/2%20dhcp.PNG)     

However, if we input do show ip dhcp binding, it won’t show any leases. This is because client dhcp requests won’t reach R1. We will configure the distribution switches as DHCP relays, which forwards clients’ broadcast messages to R1’s loopback IP address: 10.0.0.76  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part6/3%20dhcp.PNG)   

Now that those steps are completed, hosts should be able to lease IP addresses. We will check on PC2, using the command prompt. Use **ipconfig /renew** to send a dhcp discover message. If successful, ping R1’s loopback interface.  However, if we ping the ISP router at 203.0.113.1, is states Destination host unreachable. This is because NAT is not yet configured.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part6/4%20ipconfig%20renew.PNG)  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part6/5%20ISP.PNG)  

*Step 3*

