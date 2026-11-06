# Access Switches Configuration

The network has five Layer 2 access switches. Each one serves a single VLAN, places its end-device ports into that VLAN, and uses one trunk port (`Fa0/7`) as an uplink towards the core switch (Multilayer Switch1 / `Core_SW`).

## Configuration

### Switch1 - MGMT (VLAN 10)

```
hostname MGMT
!
vlan 10
 name MANAGEMENT
exit
!
interface range fa0/1 - 3
 switchport mode access
 switchport access vlan 10
exit
!
interface range fa0/7
 switchport mode trunk
exit
!
end
```

### Switch2 - HR (VLAN 20)

```
hostname HR
!
vlan 20
 name HR
exit
!
interface range fa0/1 - 3
 switchport mode access
 switchport access vlan 20
exit
!
interface range fa0/7
 switchport mode trunk
exit
!
end
```

### Switch3 - IT (VLAN 30)

```
hostname IT
!
vlan 30
 name IT
exit
!
interface range fa0/1 - 3
 switchport mode access
 switchport access vlan 30
exit
!
interface range fa0/7
 switchport mode trunk
exit
!
end
```

### Switch4 - GUEST (VLAN 40)

```
hostname GUEST
!
vlan 40
 name GUEST
exit
!
interface range fa0/1 - 3
 switchport mode access
 switchport access vlan 40
exit
!
interface range fa0/7
 switchport mode trunk
exit
!
end
```

### Switch5 - SERVERS (VLAN 50)

```
hostname SERVERS
!
vlan 50
 name SERVERS
exit
!
interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 50
exit
!
interface range fa0/7
 switchport mode trunk
exit
!
end
```

## Verification Commands

```
show vlan brief
show interfaces trunk
show interfaces status
show running-config
show mac address-table
```
