# App Server

Application server of the Bank network (Server Farm segment). It is the only server that Staff and ATM users can reach in the server farm, and it talks to the DB Server on their behalf.

## 1. Overview

| Item | Value |
|---|---|
| Device | App Server|
| Location | HQ Data Center, Server Farm (connected to Switch5) |
| Role | Core banking application portal (HTTP) |
| Subnet | 10.10.30.0/24 |

## 2. IP Configuration (Static)

| Field | Value |
|---|---|
| IPv4 Address | 10.10.30.11 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.30.1 |
| DNS Server | 10.10.10.10 |

## 3. Services

| Service | Status | Notes |
|---|---|---|
| HTTP | ON | Serves `app.bank.com` |
| HTTPS | Optional | Not required for this lab |

## 4. DNS Record

| Name | Type | Address |
|---|---|---|
| app.bank.com | A | 10.10.30.11 |

The record is hosted on the WEB Server (10.10.10.10).

## 5. Website

Home page is `index.html` . It shows the "Bank Application Portal" with modules: Account Management, Transaction Processing, ATM Service and Reports. Source file: `app-server-index.html`.

## 6. Access Control (ACL)

| Who | Access to App Server |
|---|---|
| Staff (VLAN 10) | Allowed (STAFF-IN, host 10.10.30.11) |
| ATM (VLAN 20) | Allowed (ATM-IN, host 10.10.30.11) |
| Guest (VLAN 30) | Blocked |
| DMZ (WEB, MAIL) | Allowed (DMZ-IN line 80, host 10.10.30.11) |
| Internet (ISP) | Blocked |


## 7. Connection to DB and Backup Server

The App Server is in the same subnet as the DB Server (10.10.30.10) and the Backup Server (10.10.30.12), so it reaches them directly through Switch5.

```
ping 10.10.30.10
ping 10.10.30.12
ftp 10.10.30.12
```

## 8. Testing

From a Staff PC, open the Web Browser:

```
http://app.bank.com
```

From a Guest PC, `http://app.bank.com` should fail. This is correct.
