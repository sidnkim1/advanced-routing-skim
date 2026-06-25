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

