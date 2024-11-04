### Table of Contents

 <b>Building 3-Tier LAN in Packet Tracer</b>  
  - [Introduction and Initial Setup](https://github.com/GSecAwareness/LAN/blob/main/README.md)  
  - [VLANs and Layer 2 EtherChannel](https://github.com/GSecAwareness/LAN/blob/main/part2/part2.md)
  - [IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)](https://github.com/GSecAwareness/LAN/blob/main/part3/part3.md)
  - [Rapid Spanning-Tree Protocol (RSTP)](https://github.com/GSecAwareness/LAN/blob/main/part4/part4.md)  
  - [Static and Dynamic Routing (OSPF)](https://github.com/GSecAwareness/LAN/blob/main/part5/part5.md) 
  - [Network Services: DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT](https://github.com/GSecAwareness/LAN/edit/main/part6/part6.md)
---

#### In this section, we’ll configure Open Shortest Path First (OSPF), an interior gateway protol (IGP) used in networks to find the most efficient path between nodes. By sharing routing information though a link-state protocol, OSPF creates and updates a dynamic map of the network with neighboring routers. This allows all devices to determine the best path to forward traffic. 

***Step 1*** *Configure OSPF on R1 (LAN-facing interfaces) and all Core and Distribution switches (all Layer-3 interfaces).*  

*a. Use process ID 1 and Area 0.  
b. Manually configure each device’s RID to match the loopback interface IP.  
c. On switches, use the network command to match the exact IP address of each interface.  
d. On R1, enable OSPF in interface config mode.  
e. Make sure OSPF is enabled on all loopback interfaces, too. Loopback interfaces should be passive.  
f. Each Distribution switch’s SVIs (except the Management VLAN SVI) should be passive, too.  
g. Configure all physical connections between OSPF neighbors to use a network type that doesn’t elect a DR/BDR. NOTE: This doesn’t work on the Layer-3 PortChannel interfaces between CSW1/CSW2. Leave them as the default network type.*    

