# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns used in Azure Virtual Wan for Single and Multiple Hubs
<br>
# Single vWAN Hub
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/3f08fd91-7411-48bd-b383-7436994add12)
<br>
Flow A: Spoke VM-->Route Server VIP-->SpokeVM (Reverse is the same)
<br>
Flow B: SpokeVM-->IPSEC GW--->Branch (Reverse is the same)
<br>
Flow C: Spoke VM-->MSEE PA--->Branch
<Br>
Flow D: Branch-->MSEE PA--->ExpressRoute GW--->Spoke
<Br>
Flow E: Branch CE to MSEE--->ExR Gateway--->IPSEC Gateway---->VPN Branch
<br>
Flow F: Branch VPN--->vHub IPSEC VPN--->MSEE PA---->Branch CE

If an Azure Firewall or NVA is deployed inside the vhub, that will intercept the packets in line with the route service VIP if Routing Intent is enabled. As we can see in a single hub, any traffic going to branches does not take the RS instances inside the vhub. Spoke to Spoke uses the RS instanes inside the vhub and that counts towards the vHub Infrastructure limits ~ up to 50Gbps.

# Multiple vWAN Hubs
