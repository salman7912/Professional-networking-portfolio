# Topology

| File | Description |
|------|-------------|
| `Edge-Network.pkt` | Cisco Packet Tracer lab file (open with Packet Tracer 8.x) |
| `Topology.png` | Physical and logical topology |
| `Infographic.png` | Feature overview (BGP, OSPF, HSRP, NAT, VLAN, ACL) |

## Topology

![Topology](Diagrams/Network-Topology.png)

## How to open the lab

1. Install Cisco Packet Tracer (free with a Cisco Networking Academy account).
2. Open `Edge-Network.pkt`.
3. Wait about 30 seconds for OSPF, BGP and HSRP to converge.
4. Test from PC1: `ping 8.8.8.8` and `ping 1.1.1.1`.

## Devices

- ISP-1 (AS100), ISP-2 (AS200)
- Edge-Router-1 (AS65001), Edge-Router-2 (AS65002)
- Firewall-Router, Core-Router-1, Core-Router-2
- MLS-1, MLS-2 (HSRP, inter-VLAN routing)
- Access switches, Servers switch, Customer switch
