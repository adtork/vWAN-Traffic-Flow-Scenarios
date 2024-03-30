# vWAN-Traffic-Flow-Scenarios
In this guide, we will dive into the integral components associated with vWAN traffic flows and their consequential impact on flow topology patterns. Central to the vWAN infrastructure for branches are key components such as VPN, SDWAN, and ExpressRoute. Within the vhub framework, Azure route server instances play a critical role in facilitating the traffic flows, utilizing BGP for communication. These route server instances are entrusted with intercepting traffic flows and ensuring the efficient delivery of data packets. The route server can be substituted by an Azure Firewall or Network Virtual Appliance (NVA) for packet interception and detailed inspection. The ensuing diagrams provide a visual representation of diverse flow patterns observed for single and multiple hubs within the vWAN framework. For further information on vWAN pricing, kindly refer to the provided article. [see this article](https://azure.microsoft.com/en-us/pricing/details/virtual-wan/).
<br>
# Single vWAN Hub
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/08966f1c-08ef-4e05-baaf-bd37a851f2bf)
<br>
Flow A: Spoke VM-->Route Server Instances-->Spoke VM (Reverse is the same)
<br>
Flow B: Spoke VM-->vHub VPN-->Branch VPN Concentrator
<br>
(*Reverse flow*) Branch VPN Concentrator-->vHub VPN-->Spoke VM
<br>
Flow C: Spoke VM-->MSEE Physical Address (PA)-->Branch Customer Edge (CE) (Egress flows for ExR bypass the ExR GW!)
<Br>
Flow D: Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->Spoke VM
<Br>
Flow E: Branch Customer Edge (CE)-->MSEE-->ExpressRoute GW-->vHub VPN Gateway-->Branch VPN Concentrator
<br>
Flow F: Branch VPN Concentrator-->vHub VPN--->MSEE Physical Address (PA)-->Branch Customer Edge (CE)

# Quick Take-Aways
In the aforementioned flow paths, it's pertinent to note that the Virtual Private Network (VPN) can be substituted with a Software-Defined Wide Area Network (SDWan) to yield identical results. If either an Azure Firewall or Network Virtual Appliance (NVA) is deployed within the virtual hub (vhub), they will intercept packets in lieu of the route service's Virtual IP (VIP), granted that Routing Intent is activated. This also applies to private or internet traffic via a single vHub.

During operations within a single vHub, it's important to note that flows to and from branches, whether via IPSEC, SDWan, or ExpressRoute, do not traverse the route service instances. Instead, this pattern is only observed in Spoke to Spoke flows, which contributes towards the vHub infrastructure limit, currently capped at 50Gbps per vHub, [For additional details, please refer to the linked resource](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits). Lastly, it should be mentioned that Branch to Branch flows also bypass the route service instances. 

# Multiple vWAN Hubs
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/9138efae-55d5-4f56-9345-95f66d0d3d75)
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
As observed in the single vHub scenario, the presence of a Network Virtual Appliance (NVA) or Azure Firewall (AzFW) in one or both vHubs leads to the interception of packets, diverting them from the route server instances. Similarly, a Software-Defined Wide Area Network (SDWan) would mirror the behavior of a Virtual Private Network (VPN).

In the flow paths, we note that Spoke-to-Spoke communication across hubs invariably involves the route server instances. Traffic moving from Spoke to Branch and vice versa also traverses a single set of route server instances. However, Branch-to-Branch traffic does not pass through the route server instances.

> [!NOTE]
>It's important to acknowledge that Azure Virtual WAN does not natively provide transit for ExpressRoute-to-ExpressRoute flows. To facilitate this, Global Reach is required. [For Global Reach details, please refer to the linked resource](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-global-reach). Alternatively, an Azure Firewall or NVA can be deployed in each vHub, combined with activation of routing intent and engagement with Microsoft Support. [Information on Routing Intent can be found here](https://learn.microsoft.com/en-us/azure/virtual-wan/how-to-routing-policies#expressroute).

Under this configuration, we effectively push down RFC1918 prefixes to each ExpressRoute branch, thereby providing a supernet of transit connectivity.



