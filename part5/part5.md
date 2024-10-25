### Table of Contents

 <b>Building 3-Tier LAN in Packet Tracer</b>  
  - [Introduction and Initial Setup](https://github.com/GSecAwareness/LAN/blob/main/README.md)  
  - [VLANs and Layer 2 EtherChannel](https://github.com/GSecAwareness/LAN/blob/main/part2/part2.md)
  - [IP Addressing, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP)](https://github.com/GSecAwareness/LAN/blob/main/part3/part3.md)
  - [Rapid Spanning-Tree Protocol (RSTP)](https://github.com/GSecAwareness/LAN/blob/main/part4/part4.md)  
  - [Static and Dynamic Routing (OSPF)](https://github.com/GSecAwareness/LAN/blob/main/part5/part5.md) 

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

Turn on OSPF and check the router ID. The current ID is 10.0.0.76, which is the IP address of its loopback interface. However the instructions state to manually configure the router ID and make loopback 0 a passive interface.   

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/1%20router%20id.PNG)

Enable OSPF on loopback 0 and R1’s interfaces connected to CSW1 and CSW2, which are g0/0 and g0/1. Configure the two LAN interfaces as point to point.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/2%20l0%20and%20int.PNG)  

Enable OSPF on the switches. The instructions say to active OSPF using the network command. Find the interfaces with:  
**do sh ip int brief | exclude un**  
This means do show ip interfaces brief | (pipe) exlude the unassigned interfaces. This will leave the remaining active interfaces to assign OSPF on. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/3%20network%20ospf.PNG)  

Configure point to point on the physcial ports.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/4%20psp%20csw1.PNG)  

Configure the same on CSW2.  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/5%20psp%20csw2.PNG)  

On the distribution switches, set the router id, make the loopback interface passive, and make the SVIs passive for VLAN 10, 20, and 40, but not for the management VLAN 99. A switched virtual interface (SVI) is a virtual interface that allows a Layer-3 switch to router traffic. Then activate OSPF on each active interface. Configure the point-to-point network type on g1/1/1 and g1/1/2.

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/6%20dswa2.PNG)  

**Do sh ip ospf neighbors**  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/7%20do%20sh%20neigh.PNG)  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/8%20dswb1.PNG)  

***Step 2*** *Configure one static default route for each of R1’s Internet connections. They should be recursive routes.*  

*a. Make the route via G0/1/0 a floating static route by configuring an AD value 1 greater than the default.  
b. R1 should function as an OSPF ASBR, advertising its default route to other routers in the OSPF domain.*  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/9%20do%20sh.PNG)  

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part5/10%20default%20route.PNG)  

Go on DSW-B2, a Layer-3 route, and look at the routing table   

**Do sh ip route**  

You will see the default route at the bottom of the table. It has two default routes, like it was configured.






