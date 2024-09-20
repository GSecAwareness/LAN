#### In Part Three, we’ll configure IP addresses, Layer-3 EtherChannel, and Hot Standby Router Protocol (HSRP). HSRP, a Cisco proprietary redundancy protocol, ensures automatic router failover to maintain high availability and prevent network outages. It assigns a virtual IP address to the router with the highest priority, making it the active router. If the active router goes offline, the next highest-priority router takes over. With a feature called preemption, we can configure the highest-priority router to automatically regain its active role once it’s back online. In this section, we’ll enable preemption to optimize our HSRP setup.

*1. Configure the following IP addresses on R1’s interfaces and enable them:  
    a. G0/0/0: DHCP client  
    b. G0/1/0: DHCP client  
    c. G0/0: 10.0.0.33/30  
    d. G0/1: 10.0.0.37/30  
    e. Loopback0: 10.0.0.76/32*  

Configure the interface range g0/0/0 and g0/1/0 to get their IP addresses from DHCP. Use no shutdown to enable them because router interfaces are disabled by default. 

![get-content](https://github.com/GSecAwareness/LAN/blob/main/part3/1%20dhcp.PNG)



