# Core Router Configuration (Core-Router1)

Edge router (Core-Router1) of the Bank Branch Network HQ. It connects to ISP-1, provides the HSRP default gateway (10.10.x.1) for the HQ VLANs using router-on-a-stick sub-interfaces, filters traffic with extended ACLs, and advertises a default route into OSPF.

## Configuration

```
hostname Core-Router1
!
interface GigabitEthernet0/0
 ip address 203.0.113.2 255.255.255.252
 ip access-group FROM-ISP in
!
interface GigabitEthernet0/1
 no ip address
!
interface GigabitEthernet0/1.100
 encapsulation dot1Q 100
 ip address 10.10.0.2 255.255.255.0
 standby 100 ip 10.10.0.1
 standby 100 priority 150
 standby 100 preempt
!
interface GigabitEthernet0/1.110
 encapsulation dot1Q 110
 ip address 10.10.10.2 255.255.255.0
 ip access-group DMZ-IN in
 standby 110 ip 10.10.10.1
 standby 110 priority 150
 standby 110 preempt
!
interface GigabitEthernet0/1.130
 encapsulation dot1Q 130
 ip address 10.10.30.2 255.255.255.0
 ip access-group SERVER-IN in
 standby 130 ip 10.10.30.1
 standby 130 priority 150
 standby 130 preempt
!
router ospf 1
 log-adjacency-changes
 network 10.10.0.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.255 area 0
 network 10.10.30.0 0.0.0.255 area 0
 default-information originate
!
ip classless
ip route 0.0.0.0 0.0.0.0 203.0.113.1
ip route 203.0.113.4 255.255.255.252 10.10.10.3
!
ip access-list extended FROM-ISP
 permit tcp any host 10.10.10.10 eq www
 permit tcp any host 10.10.10.10 eq 443
 permit tcp any host 10.10.10.11 eq smtp
 permit icmp any host 10.10.10.10 echo
 permit icmp any host 10.10.10.11 echo
 permit tcp any any established
 permit icmp any any echo-reply
 deny ip any any
!
ip access-list extended SERVER-IN
 permit ospf any any
 permit udp any any eq 1985
 permit tcp 10.10.30.0 0.0.0.255 any established
 permit icmp 10.10.30.0 0.0.0.255 any echo-reply
 deny ip 10.10.30.0 0.0.0.255 any
 permit ip any any
!
ip access-list extended DMZ-IN
 permit ospf any any
 permit udp any any eq 1985
 permit tcp host 10.10.10.10 eq www any
 permit tcp host 10.10.10.10 eq 443 any
 permit tcp host 10.10.10.11 eq smtp any
 permit tcp host 10.10.10.11 eq pop3 any
 permit icmp 10.10.10.0 0.0.0.255 any echo-reply
 permit ip 10.10.10.0 0.0.0.255 host 10.10.30.11
 deny ip 10.10.10.0 0.0.0.255 any
 permit ip any any
!
end
```

## Verification Commands

```
show running-config
show ip interface brief
show standby brief
show ip route
show ip ospf neighbor
show access-lists
```
