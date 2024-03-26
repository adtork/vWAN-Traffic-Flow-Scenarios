# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns used in Azure Virtual Wan.

# Intra Region -Single & Multple Hubs
![image](https://user-images.githubusercontent.com/55964102/225049336-3c26a1af-171b-45d0-824c-0e507b51c7d6.png)

○ Traffic Walk -Spoke to Spoke via vHub
<br>
•Spoke VM-->Route Service VIP-->Spoke VM (Same Flow in Reverse!)
<br>

○ Traffic Walk -Spoke to Branch/Branch to Spoke -IPSEC VPN/P2S
<br>
•Spoke VM--> VPN GW VIP-->Branch VM
<br>
•Branch VM-->VPN GW VIP-->Spoke VM

○ Traffic Walk -Spoke to Branch/Branch to Spoke -ExpressRoute
<br>
•Spoke VM-->MSEE Physical Address(PA)-->Branch VM
<br>
Branch VM-->MSEE Physical Address(PA)-->Gateway Internal IP-->Spoke VM
<br>
<br>
♦For diagram simplicity not showing MSEE routers for ExpressRoute. There would be two for each VRF!

![image](https://user-images.githubusercontent.com/55964102/224860975-a3769cd9-238a-4cfa-86bd-0e0175ea592d.png)
<br>
○ Traffic Walk -Spoke to Spoke multiple vHubs
<br>
•Spoke VM--> Route Service VIP vHub-->Route Service VIP vHub2-->Spoke VM (Same Flow in Reverse!)
<br>
○ Traffic Walk -Spoke to Branch/Branch to Spoke across vHubs
<br>
•Spoke VM-->vHub Route Service VIP
<br>
•vHub Route Service VIP--> VNG on vHub2
<br>
•VNG on vHub2 -->Branch2 VM
<br>
•Branch2 VM--> VNG on vHub2
<br>
•VNG on vHub2 -> vHub Route Service VIP
<br>
•vHub Route Service VIP-->Spoke2 VM
<br>
<br>
○ Traffic Walk -Branch to Branch across vHubs
<br>
•Branch1 VM--->VNG on vHub
<br>
•VNG on vHub-->VNG on vHub2
<br>
•VNG on vHub2-->Branch2 VM
<br>
•Branch2 VM-->VNG on vHub2
<br>
•VNG on vHub2-->VNG on vHub1
<br>
•VNG on vHub1-->Branch1 VM
<br>
<br>
♦This same flow would also work from IPSEC VNG to ExpressRoute VNG. However, ExR VNG to ExR VNG requires Global Reach. vWAN does to allow ExR to ExR transit!
<br>
<br>
○ Traffic Walk -Spoke to Branch across vHubs via ExR
<br>
•Spoke2VM--> Route Service VIP vHub
<br>
•Route Service VIP vHub-->MSEE (PA) vHub2
<br>
•MSEE (PA) vHub2 -->Branch2 VM
<br>
•Branch2 VM--> ExR GW vHub2
<br>
•ExR GW vHub2--> Route Service VIP vHub
<br>
•Route Service VIP vHub--> Spoke2 VM

