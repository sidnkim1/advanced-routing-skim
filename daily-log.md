## 6/22/26
- Read through the given documentation
- Built topology in GNS3
- Started IP addressing
<img width="901" height="808" alt="image" src="https://github.com/user-attachments/assets/b9721673-c14c-4263-aed0-00b943ce6620" />

## 6/23/26
- Finalized IP addressing scheme using 10.35.X.X structure and implemented across all routers
- Adjusted addressing to place all inter-backbone (BB) links into OSPF Area 0 for proper backbone design
- Configured all router interfaces and loopbacks in GNS3
- Implemented multi-area OSPF using interface-based configuration
- Identified and configured ABRs with proper area assignments (Area 0 + respective area)
- Applied route summarization on ABRs for Areas 1–4 and core network
- Verified OSPF neighbor adjacencies across the topology
- Validated routing tables to ensure correct propagation of routes and summaries
- Confirmed end-to-end connectivity by successfully pinging loopbacks across all routers

Notes:
- OSPF neighbors formed automatically based on matching subnet and area configuration
- Summarization working as expected (only /27 routes seen for remote areas)
- Backbone Area 0 design is now consistent and stable
- Network convergence observed after link changes

## 6/25/26
- Configured full-mesh iBGP (AS 65501) using loopback peering on all routers
- Applied update-source loopback0 and next-hop-self for all neighbors
- Configured EDGE router to advertise default route into BGP
- Troubleshot default route issues:
- Brought up NAT-facing interface (Gi0/3)
- Configured DHCP and static default route
- Enabled redistribution of static routes in BGP
- Verified all BGP neighbors are established
- Confirmed default route (0.0.0.0/0) is learned via EDGE loopback (10.255.35.1)
- Validated end-to-end connectivity (successful ping to 8.8.8.8)

Notes:
- Full-mesh iBGP required without route reflectors
- Default route must exist before BGP can advertise it
- Next-hop-self ensures proper route installation across routers

## 6/26/26
- Converted full-mesh iBGP design to route reflector-based architecture
- Selected EDGE and BB-RTR-5 as route reflectors within the topology
- Updated all router configurations to remove full-mesh peerings
- Configured route-reflector-client relationships on reflectors
- Reduced BGP neighbor count on client routers to only route reflectors
- Verified BGP session establishment after topology change
- Confirmed route reflection behavior across all routers
- Validated full routing table propagation using iBGP
- Tested end-to-end connectivity across the network

Notes:
- Route reflectors significantly reduce iBGP complexity and improve scalability
- iBGP route advertisement behavior changes when route-reflector-client is configured
- Next-hop-self remains critical for proper route installation
- Observed successful transition from full-mesh to RR design without loss of connectivity

## 6/29/26
- Completed Section 2.1: Connectivity at the Edge by integrating four CE routers into the existing transport network
- Added CE-RTR-1 through CE-RTR-4 and established physical connections to LR-AR1-1 through LR-AR4-1
- Configured eBGP peerings between CE routers (AS 65510–65513) and leaf routers (AS 65501)
- Implemented /29 transit networks from 198.51.100.0/24 for CE-to-LR connectivity
- Assigned /24 public prefixes from 100.64.0.0/10 to each CE router and configured loopback interfaces (/32)
- Created static null0 routes on CE routers to support BGP network advertisement requirements
- Advertised CE public prefixes into the iBGP domain through leaf routers
- Verified route propagation across the entire network through BGP
- Confirmed default route reception on all CE routers from leaf routers
- Validated full end-to-end connectivity between all CE routers via loopback addresses
- Corrected misconfigured BGP neighbor on LR-AR4-1 to restore proper topology

Notes:
- Reinforced understanding of eBGP vs iBGP behavior and next-hop handling
- Observed how BGP requires an exact prefix match in the RIB for advertisement
- Practiced subnetting with /29 point-to-point links for scalable edge connectivity
- Confirmed proper route propagation through route reflectors without impacting core stability

## 6/30/26
- Completed Section 2.2: Controlling BGP Prefix Advertisement and Reception
- Implemented inbound route filtering on all leaf routers to only accept assigned CE /24 prefixes
- Configured route-maps on CE-RTR-1 and CE-RTR-2 to apply no-export community to outbound routes
- Enabled community propagation across iBGP by configuring send-community on route reflectors
- Verified successful distribution of no-export community across all leaf routers
- Implemented prefix-list filtering on LR-AR3-1 and LR-AR4-1 to advertise only default routes to CE-RTR-3 and CE-RTR-4
- Confirmed that CE-RTR-3 and CE-RTR-4 only receive a default route as intended
- Validated that CE-RTR-1 and CE-RTR-2 retain selective route visibility while maintaining connectivity
- Tested end-to-end connectivity using loopback-sourced traffic to confirm policy does not break reachability

Notes:
- BGP communities require send-community to propagate across iBGP and route reflectors
- no-export affects advertisement to eBGP peers, not visibility within the AS
- Prefix filtering allows control of route visibility without impacting reachability
- Observed distinction between route visibility and forwarding capability using default routes
- Gained experience implementing policy-based routing behavior in a scalable BGP design

## 7/1/26
- 
