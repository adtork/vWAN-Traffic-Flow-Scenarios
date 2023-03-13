# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns used in Azure Virtual Wan.

# Intro
In this article, my colleague Mays and I are going to walk threw the various traffic flow scenarios with Virtual Wan. This will include both intra and interhub scenarios including SDWan, SecureHub, and Spoke/Branch flows.  

# Intra Region -Single & Multple Hubs
![image](https://user-images.githubusercontent.com/55964102/223010865-4b672dd5-f57a-4a18-ae3a-3f8649f57a99.png)

○ Traffic Walk -Spoke to Spoke via vHub
<br>
•Spoke VM--->Route Service VIP
<br>
•Route Service VIP-->Spoke VM
<br>
•Spoke VM-->Route Service VIP
<br>
•Route Service VIP-->Spoke VM

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
<br>
♦For diagram simplicity not showing MSEE routers for ExpressRoute. There would be two for each VRF!

![image](https://user-images.githubusercontent.com/55964102/224856445-c5eb75cb-d814-47cc-b569-80870e4d3eb9.png)
<br>
○ Traffic Walk -Spoke to Spoke multiple vHubs
•Spoke VM

