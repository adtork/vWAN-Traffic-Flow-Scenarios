# vWAN-Traffic-Flow-Scenarios
This guide offers an in-depth exploration of the essential elements related to vWAN traffic flows, including their significance in shaping flow topology patterns. At the heart of the vWAN branch architecture lie pivotal components such as VPN, SDWAN, and ExpressRoute. Within the vhub ecosystem, Azure route server instances hold an indispensable role in managing traffic flows, employing BGP for communication. These route server instances, concealed from the end user within the vhub, serve as the linchpin of the vWAN system. The route server instances bear the responsibility of intercepting and directing traffic flows, ensuring the smooth transmission of data packets. In a more detailed inspection scenario, these can be replaced by an Azure Firewall or Network Virtual Appliance (NVA). The moment an NVA or AzFW is incorporated into the vhub, they gain control over the data plane packet interception. The subsequent diagrams offer a pictorial depiction of the various flow patterns observed for single and multiple hubs within the vWAN infrastructure. The intent of this article is to provide a more detailed depiction of flow patterns than currently available in the Azure public documents, [see here](https://learn.microsoft.com/en-us/azure/virtual-wan/virtual-wan-global-transit-network-architecture#anytoany). For details on vWAN pricing, please refer to the [linked article](https://azure.microsoft.com/en-us/pricing/details/virtual-wan/)
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
Flow C: Spoke VM-->Route Server Instances-->MSEE Physical Address (PA)--->Branch Customer Edge (CE)
<br>
Flow D: Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->Remote Route Server Instances-->Remote Spoke VM
<br>
Flow E: Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->Remote vHub VPN GW-->Remote Branch VPN Concentrator
<br>
Flow F: Branch VPN Concentrator-->Remote Physical Address MSEE (PA)-->Remote Branch Customer Edge (CE)

# Quick Take-Aways
As observed in the single vHub scenario, the presence of a Network Virtual Appliance (NVA) or Azure Firewall (AzFW) in one or both vHubs leads to the interception of packets, diverting them from the route server instances. Similarly, a Software-Defined Wide Area Network (SDWan) would mirror the behavior of a Virtual Private Network (VPN).

In the flow paths, we note that Spoke-to-Spoke communication across hubs invariably involves the route server instances. Traffic moving from Spoke to Branch and vice versa also traverses a single set of route server instances. However, Branch-to-Branch traffic does not pass through the route server instances.

> [!NOTE]
>It's important to acknowledge that Azure Virtual WAN does not natively provide transit for ExpressRoute-to-ExpressRoute flows. To facilitate this, Global Reach is required. [For Global Reach details, please refer to the linked resource](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-global-reach). Alternatively, an Azure Firewall or NVA can be deployed in each vHub, combined with activation of routing intent and engagement with Microsoft Support. [Information on Routing Intent can be found here](https://learn.microsoft.com/en-us/azure/virtual-wan/how-to-routing-policies#expressroute).

Under this configuration, we effectively push down RFC1918 prefixes to each ExpressRoute branch, thereby providing a supernet of transit connectivity.

# Conclusion
I hoped this guide helped to furnish a more profound comprehension of packet flow dynamics within the services employed in vWAN. Armed with this understanding, users are enabled to make informed decisions regarding the initial infrastructure units, in terms of Gbps, that should be allocated to each respective vhub.



