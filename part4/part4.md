### Table of Contents

 <b>Building 3-Tier LAN in Packet Tracer</b>  
  - [Introduction and Initial Setup](https://github.com/GSecAwareness/LAN/blob/main/README.md)  
  - [VLANs and Layer 2 EtherChannel](https://github.com/GSecAwareness/LAN/blob/main/part2/part2.md)
  - [IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)](https://github.com/GSecAwareness/LAN/blob/main/part3/part3.md)
  - [Rapid Spanning-Tree Protocol (RSTP)](https://github.com/GSecAwareness/LAN/blob/main/part4/part4.md)  
  - [Static and Dynamic Routing (OSPF)](https://github.com/GSecAwareness/LAN/blob/main/part5/part5.md) 
  - [Network Services: DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT](https://github.com/GSecAwareness/LAN/edit/main/part6/part6.md)
---
#### In Part 4, we’ll configure Rapid Spanning Tree Protocol (RSTP) on our distribution and access switches. RSTP is an enhanced version of Spanning Tree Protocol (STP), designed to prevent network loops while significantly improving convergence time, allowing the network to react more quickly to changes. Although RSTP retains the same port roles as STP, it introduces three streamlined port states: Discarding, which combines STP's Blocking and Listening states; Learning, where MAC addresses are learned but traffic is not yet forwarded; and Forwarding, where traffic is actively forwarded and the port is fully operational. We’ll also enable two key features, PortFast and BPDU Guard, on our end-host ports, which will be explained in more detail later.

***Step 1*** *Configure Rapid PVST+ on all Access and Distribution switches.*  

*a. Ensure that the Root Bridge for each VLAN aligns with the HSRP Active router by configuring the lowest possible STP priority.  
b. Configure the HSRP Standby Router for each VLAN with an STP priority one increment above the lowest priority.*  

View the current spanning-tree protocol  on DSW-A1. The picture shows ieee, which is PVST+. We want to change it to RSTP.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/1%20PVST.PNG)  

To change the current mode to RSTP, use the command:
**spanning-tree mode rapid-pvst**

Do this for the remaining distribution and access switches. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/2%20RSTP.PNG)

Next, change the priority on VLAN 10 and 99 to a priority of 0, the lowest possible STP priority. Check the configuration. VLAN 10 and 99 should be the root bridge. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/3%20priority.PNG)

Next set the priority on Vlan 20 and 40 one increment above the lowest, specifically 4096.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/4%20priority%204096.PNG)

On DSW-A2, set the priority to 0 on VLAN 20 and 40

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/5%20priority.PNG)

On DSW-B1, set the VLAN priority appropriately. Do the same on DSW-B2

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/6%20priority.PNG)

***Step 2.*** *Enable PortFast and BPDU Guard on all ports connected to end hosts (including WLC1). Perform the configurations in interface config mode.*

PortFast allows ports connected to end-hosts, like PCs, to immediately move into the Forwarding state, bypassing the usual 30-second wait through the Listening and Learning states. This ensures that end-hosts get an active network connection without delay, but should only be used on ports connected directly to end devices.

BPDU Guard protects the network by disabling a port if it receives a BPDU (Bridge Protocol Data Unit). When enabled on end-host ports, it prevents unauthorized devices, such as switches, from being connected by automatically shutting down the port. Both features should be configured on interface f0/1. For interface f0/2, use the following command:
**spanning-tree portfast trunk**  

Use this command to enable portfast on f0/2 because it is a trunk port (because of WLC1). 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/7%20p%20bpdu.PNG)

Enable portfast and bpduguard on the remaining access switches. ASW-A1 was the only switch to have a trunk port because of the connection to WLC1, so don’t use the modified command. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part4/8%20p%20bpdu.PNG)














