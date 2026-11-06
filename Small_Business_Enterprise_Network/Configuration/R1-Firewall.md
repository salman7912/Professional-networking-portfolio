# R1-Firewall

R1-Firewall is the edge router/firewall of the Small Business Enterprise Network. It provides:

- Inter-VLAN routing (router-on-a-stick) for all internal VLANs
- DHCP services for Management, HR, IT and Guest VLANs
- NAT/PAT for Internet access through the ISP
- An ACL that isolates the Guest network from internal VLANs
- A default route towards the ISP

## Full Running Configuration

```
hostname R1-Firewall
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10
ip dhcp excluded-address 192.168.50.1 192.168.50.20
!
ip dhcp pool MANAGEMENT
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.50.11
ip dhcp pool HR
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 192.168.50.11
ip dhcp pool IT
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 192.168.50.11
ip dhcp pool GUEST
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 dns-server 8.8.8.8
!
interface GigabitEthernet0/0/0
 ip address 203.0.113.2 255.255.255.252
 ip nat outside
!
interface GigabitEthernet0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
!
interface GigabitEthernet0/0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip nat inside
!
interface GigabitEthernet0/0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
 ip nat inside
!
interface GigabitEthernet0/0/1.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0
 ip nat inside
 ip access-group GUEST_RESTRICTION in
!
interface GigabitEthernet0/0/1.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
 ip nat inside
!
ip nat inside source list 1 interface GigabitEthernet0/0/0 overload
ip route 0.0.0.0 0.0.0.0 203.0.113.1
!
ip access-list extended GUEST_RESTRICTION
 deny ip 192.168.40.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.30.0 0.0.0.255
 deny ip 192.168.40.0 0.0.0.255 192.168.50.0 0.0.0.255
 permit ip 192.168.40.0 0.0.0.255 any
access-list 1 permit 192.168.0.0 0.0.255.255
!
end
```

## Verification Commands

```
show running-config
show ip interface brief
show ip route
show ip dhcp binding
show ip dhcp pool
show ip nat translations
show ip nat statistics
show access-lists
ping 203.0.113.1
ping 8.8.8.8
```
