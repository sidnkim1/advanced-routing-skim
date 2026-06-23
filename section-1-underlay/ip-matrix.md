## Base Addressing

Routed Infrastructure: 10.35.X.X

Loopbacks: 10.255.35.X (1-14)


---

# LOOPBACKS (Per Router)

EDGE-RTR     → 10.255.35.1/32  
BB-RTR-1     → 10.255.35.2/32  
BB-RTR-2     → 10.255.35.3/32  
BB-RTR-3     → 10.255.35.4/32  
BB-RTR-4     → 10.255.35.5/32  
BB-RTR-5     → 10.255.35.6/32  
LR-AR1-1     → 10.255.35.7/32  
LR-AR1-2     → 10.255.35.8/32  
LR-AR2-1     → 10.255.35.9/32  
LR-AR2-2     → 10.255.35.10/32  
LR-AR3-1     → 10.255.35.11/32  
LR-AR3-2     → 10.255.35.12/32  
LR-AR4-1     → 10.255.35.13/32  
LR-AR4-2     → 10.255.35.14/32  


---

# CORE TRANSIT BLOCK

## 10.35.0.0/26  (Core + Edge shared segment)

EDGE ↔ CORE-SW → 10.35.0.1/29  
BB-RTR-1 ↔ CORE-SW → 10.35.0.2/29  
BB-RTR-2 ↔ CORE-SW → 10.35.0.3/29  
BB-RTR-3 ↔ CORE-SW → 10.35.0.4/29  
BB-RTR-4 ↔ CORE-SW → 10.35.0.5/29  
BB-RTR-5 ↔ CORE-SW → 10.35.0.6/29  

EDGE ↔ BB-RTR-1 → 10.35.0.8/30  
EDGE ↔ BB-RTR-2 → 10.35.0.12/30  

---

# AREA 1 (Top-Left Cluster)

## 10.35.1.64/27

BB-RTR-1 ↔ BB-RTR-4 → 10.35.1.64/30  

BB-RTR-1 ↔ LR-AR1-1 → 10.35.1.68/30  
BB-RTR-1 ↔ LR-AR1-2 → 10.35.1.72/30  

BB-RTR-4 ↔ LR-AR1-1 → 10.35.1.76/30  
BB-RTR-4 ↔ LR-AR1-2 → 10.35.1.80/30  

LR-AR1-1 ↔ LR-AR1-2 → 10.35.1.84/30  


---

# AREA 2 (Bottom-Left Cluster)

## 10.35.2.96/27

BB-RTR-4 ↔ BB-RTR-5 → 10.35.2.96/30  

BB-RTR-4 ↔ LR-AR2-1 → 10.35.2.100/30  
BB-RTR-4 ↔ LR-AR2-2 → 10.35.2.104/30  

BB-RTR-5 ↔ LR-AR2-1 → 10.35.2.108/30  
BB-RTR-5 ↔ LR-AR2-2 → 10.35.2.112/30  

LR-AR2-1 ↔ LR-AR2-2 → 10.35.2.116/30  


---

# AREA 3 (Bottom-Right Cluster)

## 10.35.3.128/27

BB-RTR-5 ↔ BB-RTR-3 → 10.35.3.128/30  

BB-RTR-5 ↔ LR-AR3-1 → 10.35.3.132/30  
BB-RTR-5 ↔ LR-AR3-2 → 10.35.3.136/30  

BB-RTR-3 ↔ LR-AR3-1 → 10.35.3.140/30  
BB-RTR-3 ↔ LR-AR3-2 → 10.35.3.144/30  

LR-AR3-1 ↔ LR-AR3-2 → 10.35.3.148/30  


---

# AREA 4 (Top-Right Cluster)

## 10.35.4.160/27

BB-RTR-3 ↔ BB-RTR-2 → 10.35.4.160/30  

BB-RTR-3 ↔ LR-AR4-1 → 10.35.4.164/30  
BB-RTR-3 ↔ LR-AR4-2 → 10.35.4.168/30  

BB-RTR-2 ↔ LR-AR4-1 → 10.35.4.172/30  
BB-RTR-2 ↔ LR-AR4-2 → 10.35.4.176/30  

LR-AR4-1 ↔ LR-AR4-2 → 10.35.4.180/30  


---

# OSPF SUMMARIZATION

Area 0 → 10.35.0.0/26  
Area 1 → 10.35.1.64/27  
Area 2 → 10.35.2.96/27  
Area 3 → 10.35.3.128/27  
Area 4 → 10.35.4.160/27  
