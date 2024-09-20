#### In Part Two, we’ll dive deeper into the configurations, focusing on setting up VLANs and Layer 2 EtherChannel for our two office locations. This is where we really get into the core of the network and start the building process. Most of the focus will be on the distribution and access layer switches. 

***Step 1***  *In Office A, configure a Layer-2 EtherChannel named PortChannel1 between DSW-A1 and DSW-A2 using a Cisco-proprietary protocol. Both switches should actively try to form an EtherChannel.*
   
In order to figure out the correct interfaces needed for DSW-A1, use show cdp neighbors to see the devices and  their interfaces.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/1%20sh%20cdp%20neighbors.PNG)

Create channel-group 1 and use desirable mode for pagp. Use channel-group 1 because the instructions state PortChannel1 and 1 is the group number. Desirable mode is needed because the instructions state that both switches should actively try to form an EtherChannel.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/2%20channel-group%201%20desirable.PNG)

Input the same commands into DSW-A2 and confirm the connection with:   

<b>do show etherchannel summary</b>  

If you look under the Port-Channel header, it reads Po1(SU): Port Channel 1 with the flags S = Layer 2 and U = in Use. This means we have a successful connection. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/3%20etherchannel%20summary%20desirable.PNG)


***Step 2*** *In Office B, configure a Layer-2 EtherChannel named PortChannel1 between DSW-B1 and DSW-B2 using an open standard protocol. Both switches should actively try to form an EtherChannel.*

Next, create the etherchannel between DSW-B1 and DSW-B2, according to the directions. Be mindful that it states to use an open standard protocol, specifically LACP. Use active mode so both switches actively try to form the EtherChannel. Once again, use <b>do show cdp neighbors</b> to see the neighboring devices and their interfaces.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/4%20sh%20cdp%20neighbors%20DSW-b1.PNG)

The interface range is g1/0/4-5, just like before. Once the range is inputted, create Channel-group 1 again and choose active mode.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/5%20channel-group%201%20mode%20active.PNG)

Do the same for DSW-B2, but input <b>do sh etherchannel summary</b> to confirm the active connection. Under the Port-channel header, it reads Po1(SU), indicating Port-Channel 1 is “in use” with “layer 2.” Refer to the flag table to see the different possible flags. Our flags indicate a good connection. Under the Protocol heading, it reads LACP, showing our open standard protocol, as instructed.  

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/6%20etherchannel%20summary%20active.PNG)

***Step 3.***	*Configure all links between Access and Distribution switches, including the EtherChannels, as trunk links.*  

*a) Explicitly disable DTP on all ports.
 b) Set each trunk’s native VLAN to VLAN 1000 (unused).    
 c) In Office A, allow VLANs 10, 20, 40, and 99 on all trunks.  
 d) In Office B, allow VLANs 10, 20, 30, and 99 on all trunks.*  

a)   Use “nonegotiate” to disable DTP  
b)   Change the native vlan to an unused vlan is a security measure to prevent vlan hopping attacks  

On DSW-A1, use <b>do show cdp neighbors</b> to find the see the neighboring devices and find the correct interface range.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/7%20cdp%20neighbors%20for%20A1%20A2%20A3.PNG)  

ASW-A1, A2, and A3 use the interface range G1/0/1-3 and make them trunks using <b>switchport mode trunk</b> and use <b>nonegotiate</b> to disable DTP on the ports. Set the native vlan to vlan 1000. Next, set the allowed vlan range for Office A, using 10, 20, 40, and 99. Once that is complete, repeat the steps on port-channel 1 (which is not part of the interface range)

<b>int range g1/0/1-3  
switchport mode trunk  
switchport negotiate  
switchport trunk native vlan 1000  
switchport trunk allowed vlan 10, 20, 40, 99  
Int po1  
switchport mode trunk  
switchport nonegotiate  
switchport trunk native vlan 1000  
switchport trunk allowed vlan 10, 20, 40, 99</b>  

Once complete, do the same on DSW-A2.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/8%20trunk%20nonegotiate%20native%20allowed%20dsw1%20and%202.PNG)

Now that DSW1 and 2 are out of the way, configure ASW-A1, A2, and A3. <b>Do show CDP neighbors</b> shows that interfaces g0/1-2 will be used. Repeat the process on the access switches using interface range g0/1-2.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/9%20ASW-A1%20trunk%20native%20allowed.PNG)


With Office A out of the way, we need to configure Office B in the same manner. Be mindful that Office B uses vlan 10,20,30,99 instead of Office A vlan 10,20,40,99 configuration. Starting with DSW-B1, configure the switch in the same manner. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/10%20DSW-B1%20office%20b%20config.PNG)

Next, configure the access switches in the same way. Be mindful that these switches also need vlan 10,20,30,99 just like the distribution switches. Just like Office A, there is no need to configure a port channel either. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/11%20ASW-B3%20config.PNG)

***Step 4.***	*Configure one of each office’s Distribution switches as a VTPv2 server. Use domain name JeremysITLab.*  

*a. Verify that other switches join the domain.  
b. Configure all Access switches as VTP clients.*  

First, check the VTP status and pay attention to the VTP Domain Name, like the instructions state. There is no domain name, so this will need to be changed and we should configure version 2 of VTP as well. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/12%20VTP%20status%20DSW-A1.PNG)

Once completed, DSW-A1 should send VTP advertisements, which will cause the other switches to join the domain. Check the status on another switch to verify this. In the picture below of ASW-A3, the domain is set correctly and the VTP version is set to 2. Notice the last command changed the mode to VTP client, as instructed on all access switches. Enter the configuration of ASW-A2 and ASW-A1 to set their VTP mode to client.  

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/13%20VTP%20status%20ASW-A3.PNG)

VTP advertisements are only sent out of trunk ports and the links between the distribution and core switches are not trunked. They are currently configured in access mode. Therefore, the VTP adverisements will not reach Office B. We need to configure DSW-B1 to send VTP advertisements. Issue the same commands as on DSW-A1. Check the VTP status on ASW-B3 to confirm and change the VTP mode to client. Enter the configuration of ASW-B2 and ASW-B1 to set their VTP mode to client.  

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/14%20VTP%20status%20ASW-B3.PNG)


***Step 5.*** *In Office A, create and name the following VLANs on one of the Distribution switches. Ensure that VTP propagates the changes.*    

*a. VLAN 10: PCs   
b. VLAN 20: Phones  
c. VLAN 40: Wi-Fi  
d. VLAN 99: Management*  

Because VTP is already configured on DSW-A1, the configurations should propagate to the other switches. Enter the configurations on DSW-A1 and check the configurations on ASW-A3 using <b>do show vlan brief</b>

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/15%20VLAN%20naming%20Office%20A.PNG) 

***Step 6*** *In Office B, create and name the following VLANs on one of the Distribution switches. Ensure that VTP propagates the changes.*    

*a. VLAN 10: PCs  
b. VLAN 20: Phones    
c. VLAN 30: Servers  
d. VLAN 99: Management*  

Using the same commands, enter the configurations into DSW-B1 and it will propagate the changes into the other switches. Verify the changes on ASW-B3. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/16%20VLAN%20naming%20Office%20B.PNG)

***Step 7*** *Configure each Access switch’s access port.*    

*a. LWAPs will not use FlexConnect  
b. PCs in VLAN 10, Phones in VLAN 20  
c. SRV1 in VLAN 30  
d. Manually configure access mode and explicitly disable DTP*  


ASW-A1 and ASW-B1 are both connected to LWAP (Lightweight Wireless Access Point). According to the directions, LWAPs will not use FlexConnect, so all traffic is tunneled to WLC1 in VLAN 99 Management. Therefore, the connections to the LWAPs can be access ports because they only need to support the Management VLAN. Take notice that WLC1 is in Office A, but wireless traffic from Office B also flows to WLC1 in Office A where it is assigned to VLAN 40 Wi-Fi. The end hosts are connected to f0/1 interface, so that is the interface to configure.

<b>int f0/1</b>  
<b>switchport mode access</b>	//configure the port to access mode  
<b>switchport nonegotiate</b>	//disable DTP, per instructions, even though access mode disables it by default  
<b>switchport access vlan 99</b>	//assigns the port to the management vlan  

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/17%20access%20port%20ASW-A1.PNG)

Next, use the same configurations on ASW-A2, ASW-A3, and ASW-B2, but change the vlan configurations for vlan 10 PCs and vlan 20 Phones.  

<b>int f0/1</b>  
<b>switchport mode access</b> 	//configure the port to access mode  
<b>switchport nonegotiate</b>	//disable DTP, per instructions, even though access mode disables it by default  
<b>switchport access vlan 10</b>	//assigns the port to the PCs vlan  
<b>switchport access vlan 20</b>	//assigns the port to the Phones vlan  

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/18%20access%20port%20ASW-B2.PNG)

Repeat this process for ASW-B3, changing the vlan configuration to vlan 30 for the server.

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/19%20access%20port%20ASW-B3.PNG)

***Step 8.*** *Configure ASW-A1’s connection to WLC1.*    

*a. It must support the Wi-Fi and Management VLANs.  
b. The Management VLAN should be untagged.  
c. Disable DTP.*  

Configure it as a trunk port that allows both Management VLAN and Wi-Fi VLAN with the former (Management) as the native VLAN. Use switchport nonegotiate to disable DTP. WLC1 is connected to f0/2 so that is the port to configure. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/20%20ASW-A1%20trunk%20to%20WLC1.PNG)

***Step 9*** *Administratively disable all unused ports on Access and Distribution switches.*  

Look at DSW-A1 to determine which ports to disable. Use <b>do show int status</B> In the CLI output under the Status header, the ports labeled “notconnect” should be disabled for security. Disable them all at once using the int range command. The range should be g1/0/6 -24 and g1/1/3-4. The output should look like this. Copy and paste the commands into the other distribution switches. Before writing the configuration to memory, be sure to verify the work using “do sh int status.” 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/21%20disable%20ports%20DSW-A1.PNG)  

Lastly, disable unused ports on the ASW-A1 using the same methodology. Find the unused port range and disable it. Checking  ASW-A1 reveals the “notconnect” port range to be f0/3-24. The remaining access switches are different because they only have access ports. They do not have port f0/2 connected to WLC1. Therefore, instead of f0/3-24, our new shutdown range should be f0/2-24. 

![getcontent](https://github.com/GSecAwareness/LAN/blob/main/part2/22%20ASW-A2%20shutdown%20range.PNG)

