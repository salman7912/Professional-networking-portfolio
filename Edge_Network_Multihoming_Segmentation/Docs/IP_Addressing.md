# IP Addressing

## Autonomous systems and BGP

| Device | AS | BGP router-id | Neighbors |
|--------|----|---------------|-----------|
| ISP-1 | 100 | 100.100.100.1 | 203.0.113.2 (Edge-Router-1, AS 65001), 192.0.2.2 (ISP-2, AS 200) |
| ISP-2 | 200 | 200.200.200.1 | 198.51.100.2 (Edge-Router-2, AS 65002), 192.0.2.1 (ISP-1, AS 100) |
| Edge-Router-1 | 65001 | 1.1.1.1 | 203.0.113.1 (ISP-1, AS 100), 10.0.0.2 (Edge-Router-2, AS 65002) |
| Edge-Router-2 | 65002 | 2.2.2.2 | 198.51.100.1 (ISP-2, AS 200), 10.0.0.1 (Edge-Router-1, AS 65001) |

## Point-to-point links

| Link | Network | Side A | Side B |
|------|---------|--------|--------|
| ISP-1 to Edge-Router-1 | 203.0.113.0/30 | ISP-1 Fa0/0 .1 | Edge-Router-1 Fa0/0 .2 |
| ISP-2 to Edge-Router-2 | 198.51.100.0/30 | ISP-2 Fa0/0 .1 | Edge-Router-2 Fa0/0 .2 |
| ISP-1 to ISP-2 (via Cloud) | 192.0.2.0/30 | ISP-1 Fa1/0 .1 | ISP-2 Fa1/0 .2 |
| Edge-Router-1 to Edge-Router-2 | 10.0.0.0/30 | Edge-Router-1 Fa2/0 .1 | Edge-Router-2 Fa2/0 .2 |
| Edge-Router-1 to Firewall-Router | 10.0.1.0/30 | Edge-Router-1 Fa1/0 .1 | Firewall-Router Fa0/0 .2 |
| Edge-Router-2 to Firewall-Router | 10.0.2.0/30 | Edge-Router-2 Fa1/0 .1 | Firewall-Router Fa1/0 .2 |
| Firewall-Router to Core-Router-1 | 10.0.3.0/30 | Firewall-Router Fa2/0 .1 | Core-Router-1 Fa0/0 .2 |
| Firewall-Router to Core-Router-2 | 10.0.4.0/30 | Firewall-Router Fa3/0 .1 | Core-Router-2 Fa0/0 .2 |
| Core-Router-1 to Core-Router-2 | 10.0.5.0/30 | Core-Router-1 Fa1/0 .1 | Core-Router-2 Fa1/0 .2 |
| Core-Router-1 to MLS-1 | 10.0.6.0/30 | Core-Router-1 Fa2/0 .1 | MLS-1 Fa0/7 .2 |
| Core-Router-2 to MLS-2 | 10.0.7.0/30 | Core-Router-2 Fa2/0 .1 | MLS-2 Fa0/7 .2 |
| MLS-1 to MLS-2 (Port-channel 1, LACP) | 10.0.8.0/30 | MLS-1 Po1 .1 | MLS-2 Po1 .2 |

## Interface addresses per device

| Device | Interface | Address | Connected to |
|--------|-----------|---------|--------------|
| ISP-1 | Loopback0 | 8.8.8.8/32 | Internet test address |
| ISP-1 | Fa0/0 | 203.0.113.1/30 | Edge-Router-1 |
| ISP-1 | Fa1/0 | 192.0.2.1/30 | ISP-2 (Cloud) |
| ISP-2 | Loopback0 | 1.1.1.1/32 | Internet test address |
| ISP-2 | Fa0/0 | 198.51.100.1/30 | Edge-Router-2 |
| ISP-2 | Fa1/0 | 192.0.2.2/30 | ISP-1 (Cloud) |
| Edge-Router-1 | Fa0/0 | 203.0.113.2/30 | ISP-1 (NAT outside) |
| Edge-Router-1 | Fa1/0 | 10.0.1.1/30 | Firewall-Router (NAT inside) |
| Edge-Router-1 | Fa2/0 | 10.0.0.1/30 | Edge-Router-2 (NAT inside) |
| Edge-Router-2 | Fa0/0 | 198.51.100.2/30 | ISP-2 (NAT outside) |
| Edge-Router-2 | Fa1/0 | 10.0.2.1/30 | Firewall-Router (NAT inside) |
| Edge-Router-2 | Fa2/0 | 10.0.0.2/30 | Edge-Router-1 (NAT inside) |
| Firewall-Router | Fa0/0 | 10.0.1.2/30 | Edge-Router-1 |
| Firewall-Router | Fa1/0 | 10.0.2.2/30 | Edge-Router-2 |
| Firewall-Router | Fa2/0 | 10.0.3.1/30 | Core-Router-1 |
| Firewall-Router | Fa3/0 | 10.0.4.1/30 | Core-Router-2 |
| Core-Router-1 | Fa0/0 | 10.0.3.2/30 | Firewall-Router |
| Core-Router-1 | Fa1/0 | 10.0.5.1/30 | Core-Router-2 |
| Core-Router-1 | Fa2/0 | 10.0.6.1/30 | MLS-1 |
| Core-Router-2 | Fa0/0 | 10.0.4.2/30 | Firewall-Router |
| Core-Router-2 | Fa1/0 | 10.0.5.2/30 | Core-Router-1 |
| Core-Router-2 | Fa2/0 | 10.0.7.1/30 | MLS-2 |
| MLS-1 | Fa0/7 | 10.0.6.2/30 | Core-Router-1 |
| MLS-1 | Po1 (Fa0/20 to Fa0/22) | 10.0.8.1/30 | MLS-2 |
| MLS-2 | Fa0/7 | 10.0.7.2/30 | Core-Router-2 |
| MLS-2 | Po1 (Fa0/20 to Fa0/22) | 10.0.8.2/30 | MLS-1 |

## OSPF router IDs

OSPF process 1, area 0. VLAN interfaces are passive.

| Device | Router ID |
|--------|-----------|
| Edge-Router-1 | 1.1.1.1 | 
| Edge-Router-2 | 2.2.2.2 | 
| Firewall-Router | 3.3.3.3 |
| Core-Router-1 | 4.4.4.4 | |
| Core-Router-2 | 5.5.5.5 | |
| MLS-1 | 6.6.6.6 | |
| MLS-2 | 7.7.7.7 | |

## VLAN interfaces and HSRP

| VLAN | Name | Network | HSRP virtual IP | MLS-1 | MLS-2 | HSRP active |
|------|------|---------|-----------------|-------|-------|-------------|
| 10 | Residential | 192.168.10.0/24 | 192.168.10.1 | .2 (priority 110) | .3 | MLS-1 |
| 20 | Business | 192.168.20.0/24 | 192.168.20.1 | .2 | .3 (priority 110) | MLS-2 |
| 30 | VoIP | 192.168.30.0/24 | 192.168.30.1 | .2 (priority 110) | .3 | MLS-1 |
| 40 | Gaming | 192.168.40.0/24 | 192.168.40.1 | .2 | .3 (priority 110) | MLS-2 |
| 50 | IoT | 192.168.50.0/24 | 192.168.50.1 | .2 (priority 110) | .3 | MLS-1 |
| 60 | Guest-WiFi | 192.168.60.0/24 | 192.168.60.1 | .2 | .3 (priority 110) | MLS-2 |
| 70 | Servers-Mgmt | 10.10.50.0/24 | 10.10.50.1 | .2 (priority 110) | .3 | MLS-1 |
| 100 | Customer-A | 192.168.100.0/24 | 192.168.100.1 | .2 (priority 110) | .3 | MLS-1 |
| 200 | Customer-B | 192.168.200.0/24 | 192.168.200.1 | .2 | .3 (priority 110) | MLS-2 |

The switch without a priority value uses the default of 100. Both switches use `preempt`.

Spanning tree root (Rapid-PVST) follows the HSRP active switch:

| Switch | VLANs | STP priority |
|--------|-------|--------------|
| MLS-1 | 10, 30, 50, 70, 100 | 4096 (8192 for the other VLANs) |
| MLS-2 | 20, 40, 60, 200 | 4096 (8192 for the other VLANs) |

## Servers and management hosts

All hosts are in VLAN 70 (10.10.50.0/24, mask 255.255.255.0, gateway 10.10.50.1).

| Host | IP address | DNS | Purpose |
|------|-----------|-----|---------|
| DHCP + DNS server | 10.10.50.12 | 10.10.50.12 | DHCP pools for all user VLANs, DNS |
| Syslog server | 10.10.50.10 | 10.10.50.12 | Central log collection |
| NMS | 10.10.50.11 | 10.10.50.12 | SNMP monitoring |
| Admin PC | 10.10.50.20 | 10.10.50.12 | Only host allowed to SSH to devices (ACL 99) |
| Servers switch (management) | 10.10.50.27 | | Switch-7, VLAN 70 management interface |

User, guest and customer PCs get their address from DHCP.

## Internet test addresses

| Device | Loopback0 | Reached through |
|--------|-----------|-----------------|
| ISP-1 | 8.8.8.8/32 | Edge-Router-1 |
| ISP-2 | 1.1.1.1/32 | Edge-Router-2 |

## DHCP pools

Pools are on the DHCP + DNS server (10.10.50.12). Each VLAN interface relays with `ip helper-address 10.10.50.12`.

| Pool | Default gateway | DNS | Start IP | Mask |
|------|-----------------|-----|----------|------|
| VLAN10 | 192.168.10.1 | 10.10.50.12 | 192.168.10.10 | 255.255.255.0 |
| VLAN20 | 192.168.20.1 | 10.10.50.12 | 192.168.20.10 | 255.255.255.0 |
| VLAN30 | 192.168.30.1 | 10.10.50.12 | 192.168.30.10 | 255.255.255.0 |
| VLAN40 | 192.168.40.1 | 10.10.50.12 | 192.168.40.10 | 255.255.255.0 |
| VLAN50 | 192.168.50.1 | 10.10.50.12 | 192.168.50.10 | 255.255.255.0 |
| VLAN60 | 192.168.60.1 | 10.10.50.12 | 192.168.60.10 | 255.255.255.0 |
| CustA | 192.168.100.1 | 10.10.50.12 | 192.168.100.10 | 255.255.255.0 |
| CustB | 192.168.200.1 | 10.10.50.12 | 192.168.200.10 | 255.255.255.0 |

## NAT

PAT (overload) on both edge routers. Access list 1 permits 192.168.0.0/16 and 10.0.0.0/8.

| Router | Inside | Outside | Translated address |
|--------|--------|---------|--------------------|
| Edge-Router-1 | Fa1/0, Fa2/0 | Fa0/0 | 203.0.113.2 |
| Edge-Router-2 | Fa1/0, Fa2/0 | Fa0/0 | 198.51.100.2 |

## Device management addresses

SSH is allowed only from the Admin PC (10.10.50.20).

| Device | SSH target |
|--------|-----------|
| Edge-Router-1 | 10.0.1.1 |
| Edge-Router-2 | 10.0.2.1 |
| Firewall-Router | 10.0.3.1 |
| Core-Router-1 | 10.0.3.2 |
| Core-Router-2 | 10.0.4.2 |
| MLS-1 | 10.10.50.2 |
| MLS-2 | 10.10.50.3 |
| Servers switch (Switch-7) | 10.10.50.27 |
