# Branch Router Configuration (Branch2-Router / SYL-BR)

Router of Branch 2 (Sylhet, 10.30.0.0/24). It connects the branch to the HQ core switches with two routed /30 links, serves the Staff, ATM and Guest VLANs using router-on-a-stick sub-interfaces, gives IP addresses with DHCP, filters traffic with extended ACLs, and exchanges routes with HQ using OSPF.

## Configuration

```
hostname SYL-BR
!
ip dhcp excluded-address 10.30.10.1 10.30.10.10
ip dhcp excluded-address 10.30.20.1 10.30.20.10
ip dhcp excluded-address 10.30.30.1 10.30.30.10
!
ip dhcp pool SYL-STAFF
 network 10.30.10.0 255.255.255.0
 default-router 10.30.10.1
 dns-server 8.8.8.8
ip dhcp pool SYL-ATM
 network 10.30.20.0 255.255.255.0
 default-router 10.30.20.1
 dns-server 8.8.8.8
ip dhcp pool SYL-GUEST
 network 10.30.30.0 255.255.255.0
 default-router 10.30.30.1
 dns-server 8.8.8.8
!
interface GigabitEthernet0/0
 ip address 10.0.2.1 255.255.255.252
!
interface GigabitEthernet0/1
 ip address 10.0.2.5 255.255.255.252
!
interface GigabitEthernet0/2
 no ip address
!
interface GigabitEthernet0/2.10
 encapsulation dot1Q 10
 ip address 10.30.10.1 255.255.255.0
 ip access-group STAFF-IN in
!
interface GigabitEthernet0/2.20
 encapsulation dot1Q 20
 ip address 10.30.20.1 255.255.255.0
 ip access-group ATM-IN in
!
interface GigabitEthernet0/2.30
 encapsulation dot1Q 30
 ip address 10.30.30.1 255.255.255.0
 ip access-group GUEST-IN in
!
router ospf 1
 log-adjacency-changes
 network 10.0.2.0 0.0.0.3 area 0
 network 10.0.2.4 0.0.0.3 area 0
 network 10.30.10.0 0.0.0.255 area 0
 network 10.30.20.0 0.0.0.255 area 0
 network 10.30.30.0 0.0.0.255 area 0
!
ip classless
!
ip access-list extended STAFF-IN
 permit icmp 10.30.10.0 0.0.0.255 host 10.30.10.1
 permit udp any eq bootpc any eq bootps
 permit ip 10.30.10.0 0.0.0.255 10.30.10.0 0.0.0.255
 permit ip 10.30.10.0 0.0.0.255 10.20.10.0 0.0.0.255
 permit ip 10.30.10.0 0.0.0.255 10.40.10.0 0.0.0.255
 permit ip 10.30.10.0 0.0.0.255 10.10.10.0 0.0.0.255
 permit ip 10.30.10.0 0.0.0.255 host 10.10.30.11
 deny ip any any
!
ip access-list extended ATM-IN
 permit icmp 10.30.20.0 0.0.0.255 host 10.30.20.1
 permit udp any eq bootpc any eq bootps
 permit ip 10.30.20.0 0.0.0.255 10.30.20.0 0.0.0.255
 permit ip 10.30.20.0 0.0.0.255 10.20.20.0 0.0.0.255
 permit ip 10.30.20.0 0.0.0.255 10.40.20.0 0.0.0.255
 permit ip 10.30.20.0 0.0.0.255 host 10.10.30.11
 deny ip any any
!
ip access-list extended GUEST-IN
 permit icmp 10.30.30.0 0.0.0.255 host 10.30.30.1
 permit ip 10.30.30.0 0.0.0.255 10.20.30.0 0.0.0.255
 permit ip 10.30.30.0 0.0.0.255 10.40.30.0 0.0.0.255
 permit udp any eq bootpc any eq bootps
 permit ip 10.30.30.0 0.0.0.255 10.30.30.0 0.0.0.255
 permit tcp 10.30.30.0 0.0.0.255 host 10.10.10.10 eq www
 permit tcp 10.30.30.0 0.0.0.255 host 10.10.10.10 eq 443
 permit icmp 10.30.30.0 0.0.0.255 host 10.10.10.10 echo
 deny ip any any
!
end
```

## Verification Commands

```
show running-config
show ip interface brief
show ip dhcp binding
show ip route
show ip ospf neighbor
show access-lists
```
