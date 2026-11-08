# Multilayer Switch Configuration (SW2)

Core Layer 3 switch of the Bank Branch Network HQ. It routes between the HQ VLANs (100, 110, 130) and the branch links using OSPF, and connects to SW1 with an EtherChannel (Port-channel1) over 802.1Q trunks.

## Configuration

```
hostname SW2
!
ip routing
!
interface Port-channel1
 switchport trunk allowed vlan 100,110,130
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface FastEthernet0/1
 switchport trunk allowed vlan 100,110,130
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode on
!
interface FastEthernet0/2
 switchport trunk allowed vlan 100,110,130
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode on
!
interface FastEthernet0/3
 switchport trunk allowed vlan 100,110,130
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode on
!
interface FastEthernet0/20
 no switchport
 ip address 10.0.3.6 255.255.255.252
!
interface FastEthernet0/21
 no switchport
 ip address 10.0.2.6 255.255.255.252
!
interface FastEthernet0/22
 no switchport
 ip address 10.0.1.6 255.255.255.252
!
interface FastEthernet0/23
 switchport trunk allowed vlan 100,110,130
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 100,110,130
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 130
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Vlan100
 ip address 10.10.0.5 255.255.255.0
!
interface Vlan110
 ip address 10.10.10.5 255.255.255.0
!
interface Vlan130
 ip address 10.10.30.5 255.255.255.0
!
router ospf 1
 log-adjacency-changes
 network 10.0.1.4 0.0.0.3 area 0
 network 10.0.2.4 0.0.0.3 area 0
 network 10.0.3.4 0.0.0.3 area 0
 network 10.10.0.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.255 area 0
 network 10.10.30.0 0.0.0.255 area 0
end
```

## Verification Commands

```
show running-config
show interfaces trunk
show vlan brief
show etherchannel summary
show ip interface brief
show ip ospf neighbor
show ip route
```
