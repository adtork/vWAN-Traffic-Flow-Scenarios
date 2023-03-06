# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns used in Azure Virtual Wan.

# Intro
In this article, my colleague Mays and I are going to walk threw the various traffic flow scenarios with Virtual Wan. This will include both intra and interhub scenarios including SDWan, SecureHub, and Spoke/Branch flows.  

# Intra Region -Single & Multple Hubs
![image](https://user-images.githubusercontent.com/55964102/223007871-e7794473-330e-456f-96d7-40193bc15889.png)

○ Traffic Walk -Spoke to Spoke via vHub
<br>
•Source VM--->Route Service VIP
<br>
•Route Service VIP-->Dest VM
<br>
•Dest VM-->Route Service VIP
<br>
•Route Service VIP-->Dest VM

○ Traffic Walk -Spoke to Branch-IPSEC VPN/P2S
<br>
•Spoke VM--> VPN GW VIP
<br>
•VPN GW VIP-->Branch VM
<br>
•Branch VM-->VPN GW VIP
<br>
•VPN GW VIP-->Spoke VM

○ Traffic Walk -Spoke to Branch-ExpressRoute
<br>
•Spoke VM-->MSEE Physical Address(PA)
<br>
•MSEE-->Branch VM
<br>
•Branch VM-->MSEE Physical Address(PA)
<br>
•MSEE PA--->ExR GW VIP
<br>
•ExR GW VIP-->Spoke VM
<br>
For diagram simplicity not showing MSEE routers for ExpressRoute. There would be two for each VRF!
