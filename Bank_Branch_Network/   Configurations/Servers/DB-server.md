# DB Server

Database server of the Bank network (Server Farm segment). It is reachable only through the App Server, never directly from user PCs.

## 1. Overview

| Item | Value |
|---|---|
| Device | DB Server |
| Location | HQ Data Center, Server Farm (connected to Switch5) |
| Role | Stores customer, account and transaction data (design role) |
| Subnet | 10.10.30.0/24 |

Packet Tracer has no real database software, so in this lab the DB Server is part of the topology design. It is configured for IP and reachability only.

## 2. IP Configuration (Static)

| Field | Value |
|---|---|
| IPv4 Address | 10.10.30.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.30.1 |
| DNS Server | 10.10.10.10 |

## 3. Services

| Service | Status | Notes |
|---|---|---|
| HTTP | Optional | Can hold a simple "Database Server" page |
| Other | OFF | No other service needed |

## 4. DNS Record

| Name | Type | Address |
|---|---|---|
| db.bank.com | A | 10.10.30.10 |

The record is hosted on the WEB Server (10.10.10.10).

## 5. Access Control (ACL)

| Who | Access to DB Server |
|---|---|
| Staff (VLAN 10) | Blocked |
| ATM (VLAN 20) | Blocked |
| Guest (VLAN 30) | Blocked |
| DMZ (WEB, MAIL) | Blocked (only App Server 10.10.30.11 is permitted) |
| Internet (ISP) | Blocked |
| App Server (10.10.30.11) | Allowed (same subnet) |

The DB Server can only send reply traffic. It cannot start new connections to other networks.

## 6. Testing

From the App Server (Desktop > Command Prompt):

```
ping 10.10.30.10
```

Expected result: replies received.

From a Staff PC:

```
ping 10.10.30.10
```
