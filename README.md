# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns seen in Azure Virtual Wan for Single and Multiple Hubs
<br>
# Single vWAN Hub
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/8ad7efcc-c3d1-4e39-8568-baed09bc1aaa)
<br>
Flow A: Spoke VM-->Route Server Instances-->Spoke VM (Reverse is the same)
<br>
Flow B: Spoke VM-->vHub VPN-->Branch VPN Concentrator
<br>
(*Reverse flow*) Branch VPN Concentrator-->vHub VPN-->Spoke VM
<br>
Flow C: Spoke VM-->MSEE PA-->Branch CE
<Br>
Flow D: Branch CE-->MSEE PA-->ExpressRoute GW-->Spoke VM
<Br>
Flow E: Branch CE-->MSEE-->ExpressRoute GW-->vHub VPN Gateway-->Branch VPN Concentrator
<br>
Flow F: Branch VPN Concentrator-->vHub VPN--->MSEE PA-->Branch CE

# Quick Take-Aways
In the flow paths above, VPN can also be replaced with SDWan for the same behavior! If an Azure Firewall or NVA is deployed inside the vhub,that will intercept the packets instead of the route service VIP if Routing Intent is enabled (Or Private or Internet Traffic via single vHub). In a single vHub,flows to and from branches (IPSEC/SDWan/ExR) do not go threw the route service instances.Only Spoke to Spoke flows traverse the route server instances,and that counts towards the vhub infrastructure limit (Up to 50Gbps limit per vhub), [see here](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits). Branch to Branch flows also don't go threw the route server instances. 

# Multiple vWAN Hubs
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/3ab4cc3b-8db2-4262-8666-419cbf4f3d4b)
<br>
Flow A: Spoke VM-->Route Server Instances-->Remote Route Server Instances-->Remote Spoke VM (Reverse is the same)
<br>
Flow B: Spoke VM-->Route Server Instances--->Remote vhub VPN-->Remote Branch VPN Concentrator
<br>
(*Reverse flow*) Branch VPN Concentrator-->Remote Route Server Instances-->Remote Spoke VM
<br>
Flow C: Spoke VM-->Route Server Instances-->MSEE PA--->Branch CE
<br>
Flow D: Branch CE-->MSEE PA-->ExpressRoute GW-->Remote Route Server Instances-->Remote Spoke VM
<br>
Flow E: Branch CE-->MSEE PA-->ExpressRoute GW-->Remote vHub VPN GW-->Remote Branch VPN Concentrator
<br>
Flow F: Branch VPN Concentrator-->Remote MSEE PA-->Remote Branch CE

# Quick Take-Aways
Just like above via single vhub, with an NVA or AzFW in (one vhub or both),that would intercept the packets instead of the route server instances. SDWan would behave the same as VPN, just like single vhub. In the flow paths above, we can see spoke tp spoke across hubs always takes the route server instances. Traffic from Spoke to Branch and Branch to Spoke also traverses one set of Route Server instances as well. Branch to Branch does not go threw the route server instances. Lastly, vWAN does not provide transit by default for ExprressRoute to ExpressRoute flows, that would require [Global Reach](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-global-reach). The other way to provide ExpressRoute to ExpressRoute transit is to deploy an AzFW or NVA in each vhub, turn on routing intent and engage Microsoft Support, [see here](https://learn.microsoft.com/en-us/azure/virtual-wan/how-to-routing-policies#expressroute). Under the hood we will push down RFC1918 prefixes to each ExR branch to provide a supernet of transit connectivity! 



