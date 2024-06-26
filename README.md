# vWAN-Traffic-Flow-Scenarios
This guide offers an exploration of the essential elements related to vWAN traffic flows, including their significance in shaping flow topology patterns. At the heart of the vWAN branch architecture lie pivotal components such as VPN, SDWAN, and ExpressRoute. Within the vhub ecosystem, the routing instances hold the crucial role in managing traffic flows, employing BGP for communication. These routing instances, concealed from the end user within the vhub, serve as the linchpin of the vWAN system. The routing instances bear the responsibility of intercepting and directing traffic flows, ensuring the smooth transmission of data packets. If packet flow inspetion is also required, these can be aided by an Azure Firewall or Network Virtual Appliance (NVA) inside the vHub. The moment an NVA or AzFW is incorporated into the vhub, they gain control over the data plane packet interception. To enable this inspection, Routing Intent must be enabled on multi-vhub scenarios, or Private/Internet Traffic must be enabled via Firewall policy if an Azure Firewall is deployed within the vhub(s). The subsequent diagrams offer a pictorial depiction of the various flow patterns observed for single and multiple hubs within the vWAN infrastructure. The intent of this article is to provide a more detailed depiction of flow patterns than currently available in the Azure public documents, [see here](https://learn.microsoft.com/en-us/azure/virtual-wan/virtual-wan-global-transit-network-architecture#anytoany). For details on vWAN pricing, please refer to the [linked article](https://azure.microsoft.com/en-us/pricing/details/virtual-wan/)
<br>

# Single vWAN Hub Flows
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/08966f1c-08ef-4e05-baaf-bd37a851f2bf)
<br>
| Traffic Flows  | Traffic Flow Paths |
| :------------- | :------------- |
| Flow A  | Spoke VM-->Routing Instances-->Spoke VM  |
| Flow B  | Spoke VM-->vHub VPN-->Branch VPN Concentrator  |
| Flow C  | Spoke VM-->MSEE Physical Address (PA)-->Branch Customer Edge (CE) (Egress flows for ExR bypass the ExR GW!)  |
| Flow D  | Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->Spoke VM  |
| Flow E  | Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->vHub VPN Gateway-->Branch VPN Concentrator  |
| Flow F  | Branch VPN Concentrator-->vHub VPN--->MSEE Physical Address (PA)-->Branch Customer Edge (CE)  |

# Quick Take-Aways
In the aforementioned flow paths, it's pertinent to note that the Virtual Private Network (VPN) can be substituted with a Software-Defined Wide Area Network (SDWan) tunnels to yield identical results in the diagram above. If either an Azure Firewall or Network Virtual Appliance (NVA) is deployed within the virtual hub (vhub), they will intercept packets in lieu of the route service's Virtual IP (VIP), granted that Routing Intent is activated, or Private/Internet security via Azure Firewall policy on a single vhub! 

During operations within a single vHub, it's important to realize that flows to and from branches, whether via IPSEC, SDWan, or ExpressRoute, do not traverse the route service instances. Instead, this pattern is only observed in Spoke to Spoke flows, which contributes towards the vHub infrastructure limit, currently capped at 50Gbps per vHub, [For additional details, please refer to the linked resource](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits). Lastly, it should be mentioned that Branch to Branch flows also bypass the route service instances. 

# Multiple vWAN Hub Flows
![image](https://github.com/adtork/vWAN-Traffic-Flow-Scenarios/assets/55964102/9138efae-55d5-4f56-9345-95f66d0d3d75)
<br>
| Traffic Flows  | Traffic Flow Paths |
| :------------- | :------------- |
| Flow A  | Spoke VM-->Routing Instances-->Remote Routing Instances-->Remote Spoke VM |
| Flow B  | Spoke VM-->Routing Instances--->Remote vhub VPN-->Remote Branch VPN Concentrator |
| Flow C  | Spoke VM-->Routing Instances-->Remote MSEE Physical Address (PA)--->Remote Branch Customer Edge (CE) |
| Flow D  | Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->Remote Routing Instances-->Remote Spoke VM |
| Flow E  | Branch Customer Edge (CE)-->MSEE Physical Address (PA)-->ExpressRoute GW-->Remote vHub VPN GW-->Remote Branch VPN Concentrator |
| Flow F  | Branch VPN Concentrator-->Remote Physical Address MSEE (PA)-->Remote Branch Customer Edge (CE)  |

# Quick Take-Aways
The same principles apply to multiple vhubs in terms of SDWan tunnels and Azure Firewall/NVA behavior as they do a single vHub. 

In the flow patterns for multiple vhubs, we note that Spoke-to-Spoke communication across hubs invariably involves the routing instances. Traffic moving from Spoke to Branch and vice versa also traverses a single set of routing instances. However, Branch-to-Branch traffic does not pass through the routing instances, just like single vhub behavior. 

> [!NOTE]
>It's important to acknowledge that Azure Virtual WAN does not natively provide transit for ExpressRoute-to-ExpressRoute flows. To facilitate this, Global Reach is required. [For Global Reach details, please refer to the linked resource](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-global-reach). Alternatively, an Azure Firewall or NVA can be deployed in each vHub, combined with activation of routing intent and engagement with Microsoft Support. [Information on Routing Intent can be found here to facilitate transit between ExR Circuits](https://learn.microsoft.com/en-us/azure/virtual-wan/how-to-routing-policies#expressroute).

Under this configuration, we effectively push down RFC1918 prefixes to each ExpressRoute branch, thereby providing a supernet of transit connectivity.

# Conclusion
I hoped this guide helped shed a bit more light on the comprehension of packet flow dynamics within the services employed in vWAN. Armed with this understanding, users are enabled to make informed decisions regarding the initial infrastructure units, in terms of (Gbps), that should be allocated to each respective vhub.



