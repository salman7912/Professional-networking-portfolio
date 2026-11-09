# Switch Configuration (Switch2 / SW-SYL-BR)

Access switch of Branch 2 (Sylhet, 10.30.0.0/24). It places end devices into the Staff (VLAN 10), ATM (VLAN 20) and Guest (VLAN 30) VLANs using access ports, and carries all VLANs to the branch router (SYL-BR) over an 802.1Q trunk.

## Configuration

```
hostname SW-SYL-BR
!
interface range FastEthernet0/1-3
 switchport access vlan 10
 switchport mode access
!
interface range FastEthernet0/4-6
 switchport access vlan 20
 switchport mode access
!
interface range FastEthernet0/7-9
 switchport access vlan 30
 switchport mode access
!
interface GigabitEthernet0/2
 switchport mode trunk
!
end
```

## Verification Commands

```
show running-config
show vlan brief
show interfaces trunk
show spanning-tree
```
