# Configs

Sanitized configuration of every device in the lab. Each `.txt` file is the `show running-config` of one device, with `enable secret` and `password 7` values replaced by `salman@67912`. Replace them with your own before you paste a file into a device.

## Files

| File | Device | Role | Main features |
|------|--------|------|---------------|
| [ISP-1.txt](ISP-1.txt) | ISP-1 (AS 100) | Provider router | eBGP to Edge-Router-1 and ISP-2, loopback 8.8.8.8 |
| [ISP-2.txt](ISP-2.txt) | ISP-2 (AS 200) | Provider router | eBGP to Edge-Router-2 and ISP-1, loopback 1.1.1.1 |
| [Edge-Router-1.txt](Edge-Router-1.txt) | Edge-Router-1 (AS 65001) | Edge to ISP-1 | eBGP, NAT/PAT, OSPF default originate, `ANTI-SPOOF` ACL, Syslog, SNMP |
| [Edge-Router-2.txt](Edge-Router-2.txt) | Edge-Router-2 (AS 65002) | Edge to ISP-2 | eBGP, NAT/PAT, OSPF default originate, `ANTI-SPOOF` ACL, Syslog, SNMP |
| [Firewall-Router.txt](Firewall-Router.txt) | Firewall-Router | Edge to core filter | `FROM-EDGE` ACL, OSPF, ECMP across both edges, SSH |
| [Core-Router-1.txt](Core-Router-1.txt) | Core-Router-1 | Core | OSPF to Firewall, Core-Router-2 and MLS-1 |
| [Core-Router-2.txt](Core-Router-2.txt) | Core-Router-2 | Core | OSPF to Firewall, Core-Router-1 and MLS-2 |
| [MLS-1.txt](MLS-1.txt) | MLS-1 | Distribution | VLAN SVIs, HSRP, Rapid-PVST, Port-channel 1, DHCP relay, per-VLAN ACLs |
| [MLS-2.txt](MLS-2.txt) | MLS-2 | Distribution | VLAN SVIs, HSRP, Rapid-PVST, Port-channel 1, DHCP relay, per-VLAN ACLs |
| [MLS-ACL.txt](MLS-ACL.txt) | MLS-1 and MLS-2 | Access control | All inbound ACLs, paste on both switches |
| [Access-Switches.txt](Access-Switches.txt) | Switch-1 to Switch-8 | Access | Switch-1 to Switch-6 user VLANs, Switch-7 servers (VLAN 70, SSH), Switch-8 customer switch (VLAN 100 and 200, hostname `Customer`) |

## Paste order

If you rebuild the lab, configure the devices from the outside in, and test each layer before the next one.

1. ISP-1, ISP-2
2. Edge-Router-1, Edge-Router-2
3. Firewall-Router
4. Core-Router-1, Core-Router-2
5. MLS-1, MLS-2 (VLANs, SVIs, HSRP, EtherChannel, OSPF)
6. Access switches, Servers switch, Customer switch
7. [MLS-ACL.txt](MLS-ACL.txt) on both MLS switches, last, after DHCP and Internet work

Applying the ACLs last makes it easy to tell a routing fault from an ACL block.

## MLS-ACL.txt

The file has three steps, and the same file is pasted on MLS-1 and MLS-2.

| Step | What it does |
|------|--------------|
| 1 | Creates `RES-IN`, `BIZ-IN`, `VOIP-IN`, `GAME-IN`, `IOT-IN`, `GUEST-IN`, `CUSTA-IN`, `CUSTB-IN` and `SERVER-IN` |
| 2 | Applies each ACL inbound on its VLAN interface, then saves |

| ACL | VLAN | Allows | Blocks |
|-----|------|--------|--------|
| `RES-IN`, `BIZ-IN`, `VOIP-IN`, `GAME-IN` | 10, 20, 30, 40 | DHCP, DNS, ping to the DNS server and own gateway, Internet, replies to Admin PC and NMS | Other VLANs, other servers |
| `IOT-IN` | 50 | DHCP, DNS, ping to own gateway, Internet, replies to Admin PC and NMS | Ping to DNS server, other VLANs, other servers |
| `GUEST-IN`, `CUSTA-IN`, `CUSTB-IN` | 60, 100, 200 | DHCP, DNS, ping to own gateway, Internet | Everything private, including Admin PC and NMS |
| `SERVER-IN` | 70 | HSRP hello, all traffic from Admin PC, SNMP and ping from NMS, DHCP and DNS replies | Everything else (Syslog and NMS cannot reach the Internet) |



Check the result with:

```
show access-lists
show ip interface vlan < > | include access list
```

The match counters should increase on the `deny ... 10.0.0.0` lines when a user VLAN tries to reach a private range.

## SSH management

Devices that allow management use SSH version 2, a local account and a VTY ACL for the Admin PC only:

```
ip domain-name lab.local
username admin secret 67991212
crypto key generate rsa general-keys modulus 1024
ip ssh version 2
access-list 99 permit host 10.10.50.20
line vty 0 4
 login local
 access-class 99 in
 transport input ssh
```

Create the `username` before you change the VTY lines to `login local`. The console is not affected, so you can recover from a mistake.

## Notes

- Save with `write memory` on the device, and save the `.pkt` file with Ctrl+S in Packet Tracer. The configuration reverts when you reopen an unsaved `.pkt`.
- Packet Tracer rejects `logging trap`, `snmp-server host ... version 2c` and `transport input ssh telnet`. Those commands are not used in these files.
- Addresses and VLAN numbers are listed in [../Docs/IP_Addressing.md](../Docs/IP_Addressing.md).
