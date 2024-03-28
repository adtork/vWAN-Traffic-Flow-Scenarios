# vWAN-Traffic-Flow-Scenarios
A simple walk threw of various traffic patterns seen in Azure Virtual Wan for Single and Multiple Hubs
<br>
# Single vWAN Hub
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/d539e04d-07a7-4432-a7a2-2c7646357d70)
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
In the aforementioned flow paths, it's pertinent to note that the Virtual Private Network (VPN) can be substituted with a Software-Defined Wide Area Network (SDWan) to yield identical results. If either an Azure Firewall or Network Virtual Appliance (NVA) is deployed within the virtual hub (vhub), they will intercept packets in lieu of the route service's Virtual IP (VIP), granted that Routing Intent is activated. This also applies to private or internet traffic via a single vHub.

During operations within a single vHub, it's important to note that flows to and from branches, whether via IPSEC, SDWan, or ExpressRoute, do not traverse the route service instances. Instead, this pattern is only observed in Spoke to Spoke flows, which contributes towards the vHub infrastructure limit, currently capped at 50Gbps per vHub, [For additional details, please refer to the linked resource](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits). Lastly, it should be mentioned that Branch to Branch flows also bypass the route service instances. 

# Multiple vWAN Hubs
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/158928e4-6d2d-4663-a7fa-4c197decb3dc)
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



