# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns used in Azure Virtual Wan for Single and Multiple Hubs
<br>
# Single vWAN Hub
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/0d14b614-cb3f-4cb7-9298-6a45c3c3706b)
<br>
Flow A: Spoke VM-->Route Server Instances-->SpokeVM (Reverse is the same)
<br>
Flow B: SpokeVM-->VPN GW--->Branch VPN Concentrator (Reverse is the same)
<br>
Flow C: Spoke VM-->MSEE PA--->Branch CE
<Br>
Flow D: Branch CE-->MSEE PA--->ExpressRoute GW--->Spoke VM
<Br>
Flow E: Branch CE-->MSEE--->ExR Gateway--->VPN Gateway---->VPN Branch
<br>
Flow F: Branch VVPN Concentrator--->vHub VPN--->MSEE PA---->Branch CE

If an Azure Firewall or NVA is deployed inside the vhub, that will intercept the packets in line with the route service VIP if Routing Intent is enabled. In a single vHub, flows to and from branches do not go threw the route service instances. Only Spoke to Spoke flows go traverse the route server instances, and that counts towards the vhub infrastructure limit (Up to 50Gbps)

# Multiple vWAN Hubs

![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/552db9bb-71d2-4028-80db-17686462d66f)

