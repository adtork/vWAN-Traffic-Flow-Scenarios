# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns used in Azure Virtual Wan for Single and Multiple Hubs
<br>
# Single vWAN Hub
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/e99be9fa-4f40-49e5-8bed-eeab2bbd53cf)

Flow A: Spoke VM-->Route Server VIP-->SpokeVM (Reverse is the same)
<br>
Flow B: SpokeVM-->IPSEC GW--->Branch (Reverse is the same)
<br>
Flow C: Spoke VM-->MSEE PA--->Branch
<Br>
Flow D: Branch-->MSEE PA--->ExpressRoute GW--->Spoke

If an Azure Firewall or NVA was in the vHub that what intercept the packets in line with the route service VIP. As we can see in a single hub, any traffic going to branches does not take the RS instances inside the vhub. Spoke to Spoke uses the RS instanes inside the vhub and counts towards the vHub Infrastructure limits ~ up to 50Gbps.

# Multiple vWAN Hubs
