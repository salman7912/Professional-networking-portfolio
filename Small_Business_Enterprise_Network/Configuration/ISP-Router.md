# ISP Router Configuration

The ISP router simulates the Internet Service Provider. It connects to R1-Firewall over a `/30` point-to-point link and uses a loopback interface (`8.8.8.8`) to simulate an Internet destination (such as a public DNS server).

## 

## Full Running Configuration

```
hostname ISP
!
interface Loopback0
 ip address 8.8.8.8 255.255.255.255
!
interface GigabitEthernet0/0/0
 ip address 203.0.113.1 255.255.255.252
!
end
```

## Verification Commands

```
show running-config
show ip interface brief
show ip route
ping 203.0.113.2
```
