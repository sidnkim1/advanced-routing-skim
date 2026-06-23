enable
configure terminal

hostname BB-RTR-2

enable secret cisco

! --- Local Access ---
line con 0
  logging synchronous
  login local
  exit

line vty 0 4
  logging synchronous
  login local
  exit

! --- Loopback ---
interface loopback0
  ip address 10.255.35.3 255.255.255.255
  ip ospf 100 area 0

! --- OSPF Process ---
router ospf 100
  router-id 10.255.35.3
 
  ! Area 0 summary (core)
  area 0 range 10.35.0.0 255.255.255.192
 
  ! Area 4 summary (top-right cluster)
  area 4 range 10.35.4.160 255.255.255.224

! --- Interfaces ---
interface g0/0
  description to_core
  ip address 10.35.0.3 255.255.255.248
  ip ospf 100 area 0
  no shutdown

interface g0/1
  description to_edge
  ip address 10.35.0.13 255.255.255.252
  ip ospf 100 area 0
  no shutdown

interface g0/2
  description to_BB-RTR-3
  ip address 10.35.4.161 255.255.255.252
  ip ospf 100 area 0
  no shutdown

interface g0/3
  description to_LR-AR4-1
  ip address 10.35.4.173 255.255.255.252
  ip ospf 100 area 4
  no shutdown

interface g0/4
  description to_LR-AR4-2
  ip address 10.35.4.177 255.255.255.252
  ip ospf 100 area 4
  no shutdown

end
write memory
