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

In the flow paths above, VPN can also be replaced with SDWan for the same behavior! If an Azure Firewall or NVA is deployed inside the vhub, that will intercept the packets instead of the route service VIP if Routing Intent is enabled (Or Private or Internet Traffic via single vHub). In a single vHub, flows to and from branches (IPSEC/SDWan/ExR) do not go threw the route service instances. Only Spoke to Spoke flows traverse the route server instances, and that counts towards the vhub infrastructure limit (Up to 50Gbps limit per vhub)

# Multiple vWAN Hubs
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/3ab4cc3b-8db2-4262-8666-419cbf4f3d4b)




