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


## 7/6/26
- Began Section 3.1: Overlay Technologies – Policy-Based IPsec Tunnels
- Selected CE-RTR-1 and CE-RTR-2 as IPsec tunnel endpoints
- Advertised WAN transit networks (198.51.100.0/29 and 198.51.100.8/29) into BGP using network statements
- Modified inbound BGP filters on LR-AR1-1 and LR-AR2-1 to permit advertisement of CE WAN prefixes
- Refreshed BGP sessions and verified successful receipt of new prefixes on leaf routers
- Validated propagation of WAN prefixes throughout AS 65501 using BGP routing table verification
- Confirmed end-to-end reachability between IPsec peer addresses (198.51.100.2 and 198.51.100.10)
- Created internal LAN segments behind CE-RTR-1 and CE-RTR-2 using 10.1.1.0/24 and 10.2.2.0/24 networks
- Added interface descriptions and documentation comments throughout router configurations
- Deployed and configured VPCS hosts behind both CE routers
- Verified local host-to-gateway connectivity at each site
- Confirmed RFC1918 networks were not reachable across the provider network prior to tunnel deployment
- Configured ISAKMP Phase 1 policies using AES encryption, SHA hashing, pre-shared key authentication, and DH Group 2
- Implemented Phase 2 IPsec transform sets using ESP-AES and ESP-SHA-HMAC
- Created interesting traffic ACLs to identify traffic between internal LANs for encryption
- Built crypto maps defining tunnel peers, transform sets, and protected traffic
- Applied crypto maps to CE WAN interfaces and configured static routes to remote protected networks
- Initiated tunnel establishment by generating traffic between internal hosts
- Verified successful IPsec tunnel negotiation and Security Association establishment
- Confirmed QM_IDLE ISAKMP state and ACTIVE(ACTIVE) inbound and outbound IPsec Security Associations
- Validated encryption and decryption counters were incrementing during host-to-host communication
- Successfully established secure connectivity between 10.1.1.0/24 and 10.2.2.0/24 across the transport network
- Performed traceroute testing and observed encrypted traffic behavior across the IPsec overlay

Notes:
- Policy-based IPsec uses crypto maps and ACLs rather than tunnel interfaces
- Transport network provides BGP reachability while IPsec functions as an encrypted overlay
- RFC1918 routes remain isolated from the provider network and are transported securely through the tunnel
- Initial ping loss is expected while Phase 1 and Phase 2 negotiations complete
- Successful tunnel operation is verified through QM_IDLE state, ACTIVE Security Associations, and increasing encrypt/decrypt counters
- Observed how IPsec creates an overlay network on top of the existing OSPF/iBGP transport infrastructure

! Route Reflector Routes (at this stage)

<img width="659" height="370" alt="image" src="https://github.com/user-attachments/assets/1da99cbc-8a4e-481d-8d73-deef8a50f8d8" />

! Host PC traffic through tunnel

<img width="726" height="427" alt="image" src="https://github.com/user-attachments/assets/3fafc57e-261f-4d21-b6af-08620ea0a762" />

! CE-RTR-1 crypto/tunnel details

<img width="690" height="765" alt="image" src="https://github.com/user-attachments/assets/f83b2cf9-0ac4-4bd3-b0a4-2033a2960c29" />


## 7/9/26
- Completed Section 3.2: Hub and Spoke IPsec with IGP Routing
- Removed the policy-based IPsec tunnel configuration from Section 3.1 on CE-RTR-1 and CE-RTR-2
- Validated removal of existing Security Associations and confirmed a clean IPsec state before deployment
- Designated CE-RTR-1 as the IPsec Hub and CE-RTR-2 through CE-RTR-4 as Spokes
- Added a second internal LAN segment to each CE router to support multi-subnet route advertisement
- Configured IPsec keyrings, ISAKMP policies, transform sets, and IPsec profiles on the Hub router
- Implemented a Virtual-Template on CE-RTR-1 to dynamically create Virtual-Access interfaces for spoke connections
- Configured Tunnel0 interfaces on CE-RTR-2, CE-RTR-3, and CE-RTR-4 using IP unnumbered Loopback0 addressing
- Established dynamic route-based IPsec tunnels from all spokes to the Hub router
- Troubleshot spoke connectivity issues caused by missing advertisement and filtering of CE-RTR-3 and CE-RTR-4 WAN transit networks
- Modified BGP advertisements and prefix filters to allow transit reachability for all hub-and-spoke tunnel endpoints
- Verified successful tunnel establishment by confirming Tunnel0 interfaces were up/up on all spokes
- Verified dynamic creation of Virtual-Access interfaces on the Hub router for each connected spoke
- Confirmed three active ISAKMP peer relationships and successful IPsec Security Associations for all spoke tunnels
- Enabled EIGRP ASN 100 across the IPsec overlay network
- Advertised loopback interfaces and internal LAN subnets into EIGRP from all CE routers
- Verified EIGRP neighbor adjacencies formed between the Hub and all spoke routers across Virtual-Access interfaces
- Added and configured two VPCS hosts behind each CE router to activate all internal LAN segments
- Validated EIGRP route propagation between all sites and confirmed remote subnet learning across the overlay
- Verified spoke-to-spoke communication by successfully transmitting traffic between hosts located behind different CE sites
- Documented end-to-end forwarding behavior demonstrating how traffic is decrypted, routed by the Hub, re-encrypted, and forwarded to the destination spoke

Notes:
- Route-based IPsec uses Tunnel interfaces and IPsec profiles instead of crypto maps and interesting traffic ACLs
- Virtual-Templates dynamically generate Virtual-Access interfaces when spokes connect to the Hub
- EIGRP runs across the encrypted overlay and automatically distributes remote LAN reachability information
- The transport network only forwards encrypted IPsec traffic and has no knowledge of internal RFC1918 networks
- Hub-and-spoke topology significantly reduces tunnel count compared to full-mesh VPN deployments
- Spoke-to-spoke communication is achieved by routing traffic through the Hub, where packets are decrypted, routed, and re-encrypted toward the destination spoke

<img width="705" height="786" alt="image" src="https://github.com/user-attachments/assets/e24287f1-2f1c-47d0-927c-1c253d2b6043" />

<img width="530" height="265" alt="image" src="https://github.com/user-attachments/assets/ab6d0cf1-12e2-4cd4-b3f7-55169ff7f3e5" />
