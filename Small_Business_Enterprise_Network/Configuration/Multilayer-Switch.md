# Multilayer Switch Configuration (Core_SW)

Core Layer 3 switch of the Small Business Enterprise Network. It connects the access switches and the Guest Wi-Fi access point using 802.1Q trunks.

## Configuration

```
hostname Core_SW
!
interface FastEthernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface FastEthernet0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface FastEthernet0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface FastEthernet0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface FastEthernet0/5
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface FastEthernet0/6
 switchport access vlan 40
 switchport trunk encapsulation dot1q
 switchport mode access
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 10,20,30,40,50
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
end
```

## Verification Commands

```
show running-config
show interfaces trunk
show vlan brief
