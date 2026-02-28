# Lab 03 — VLAN Configuration & Trunking

**Exam Objectives:** 2.1, 2.2  
**Status:** ✅ Complete  
**Tool:** Cisco Packet Tracer

---

## Scenario

Configure a small campus network with three departments — Sales, IT, and Management — each isolated in their own VLAN. Two switches must share VLAN traffic across a trunk link. A router provides inter-VLAN routing via router-on-a-stick.

---

## Topology

```
         [Router R1]
              |
         (Gi0/0 trunk)
              |
         [Switch SW1] ─────── [Switch SW2]
         (trunk link)         (trunk link)
         /        \                  |
   Fa0/1(VLAN10)  Fa0/2(VLAN20)  Fa0/1(VLAN30)
   [PC-Sales]     [PC-IT]         [PC-Mgmt]
   10.10.10.10    10.10.20.10     10.10.30.10
```

---

## Objectives

1. Create VLANs 10, 20, and 30 on both switches
2. Assign access ports to correct VLANs
3. Configure trunk links between switches and between SW1 and the router
4. Configure router subinterfaces for inter-VLAN routing
5. Verify all PCs can ping their default gateway
6. Verify inter-VLAN routing works between departments

---

## Configuration

### Step 1 — Create VLANs on SW1 and SW2

```
SW1(config)# vlan 10
SW1(config-vlan)# name SALES
SW1(config)# vlan 20
SW1(config-vlan)# name IT
SW1(config)# vlan 30
SW1(config-vlan)# name MANAGEMENT
```

*Repeat on SW2.*

### Step 2 — Assign Access Ports

```
! SW1 - Sales PC
SW1(config)# interface fa0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10

! SW1 - IT PC
SW1(config)# interface fa0/2
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 20

! SW2 - Management PC
SW2(config)# interface fa0/1
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 30
```

### Step 3 — Configure Trunk Links

```
! SW1 uplink to Router
SW1(config)# interface gi0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,30,99

! SW1 to SW2 link
SW1(config)# interface gi0/2
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,30,99
```

### Step 4 — Router Subinterfaces (Router-on-a-Stick)

```
R1(config)# interface gi0/0
R1(config-if)# no shutdown

R1(config)# interface gi0/0.10
R1(config-subif)# encapsulation dot1q 10
R1(config-subif)# ip address 10.10.10.1 255.255.255.0

R1(config)# interface gi0/0.20
R1(config-subif)# encapsulation dot1q 20
R1(config-subif)# ip address 10.10.20.1 255.255.255.0

R1(config)# interface gi0/0.30
R1(config-subif)# encapsulation dot1q 30
R1(config-subif)# ip address 10.10.30.1 255.255.255.0
```

---

## Verification

### Verify VLANs exist and ports are assigned correctly
```
SW1# show vlan brief

VLAN  Name       Status    Ports
1     default    active    
10    SALES      active    Fa0/1
20    IT         active    Fa0/2
30    MANAGEMENT active    
```

### Verify trunk is up and carrying correct VLANs
```
SW1# show interfaces trunk

Port   Mode   Encapsulation  Status   Native vlan
Gi0/1  on     802.1q         trunking 99
Gi0/2  on     802.1q         trunking 99

VLANs allowed and active in management domain: 10,20,30,99
```

### Verify inter-VLAN routing (ping from Sales to IT)
```
PC-Sales> ping 10.10.20.10
Reply from 10.10.20.10: bytes=32 time<1ms TTL=127
Reply from 10.10.20.10: bytes=32 time<1ms TTL=127
Success rate is 100 percent (4/4)
```

---

## Lessons Learned

**Native VLAN mismatch** — Forgot to set native VLAN 99 on both ends of the trunk initially. CDP threw a native VLAN mismatch warning. Fixed by setting `switchport trunk native vlan 99` on both sides.

**`encapsulation dot1q` before `ip address`** — Tried to assign the IP address before setting encapsulation on the subinterface. IOS rejected it. Order matters: encapsulation must come first.

**Physical interface must be `no shutdown`** — The subinterfaces wouldn't come up until the parent `gi0/0` interface was enabled. Easy to miss.

---

## Homelab Connection

This lab directly maps to my pfSense homelab setup. pfSense handles inter-VLAN routing the same way this router does — each VLAN has a gateway IP on the firewall. The key difference is pfSense adds firewall rules to control what traffic is actually *permitted* between VLANs, while this basic lab allows all inter-VLAN traffic. In production, you'd always add ACLs or firewall rules on top of the routing.
