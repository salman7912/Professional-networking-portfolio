# WEB Server

Public-facing web and DNS server of the Bank network (DMZ segment).

## 1. Overview

| Item | Value |
|---|---|
| Device | WEB Server |
| Location | HQ Data Center, DMZ (connected to Switch4) |
| Role | HTTP/HTTPS website, internal DNS |
| Domain | bank.com |

## 2. IP Configuration (Static)

| Field | Value |
|---|---|
| IPv4 Address | 10.10.10.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.10.1 |
| DNS Server | 10.10.10.10 |

## 3. Services

| Service | Status | Notes |
|---|---|---|
| HTTP | ON | Serves `www.bank.com` |
| HTTPS | ON | Port 443 |
| DNS | ON | Internal DNS for all branches |

## 4. DNS Records

| Name | Type | Address |
|---|---|---|
| www.bank.com | A | 10.10.10.10 |
| mail.bank.com | A | 10.10.10.11 |
| app.bank.com | A | 10.10.30.11 |
| db.bank.com | A | 10.10.30.10 |

All DHCP pools (Staff, ATM, Guest) hand out `10.10.10.10` as the DNS server.

## 5. Website

Home page is `index.html` .

## 6. Access Control (ACL)

| Who | Access to WEB Server |
|---|---|
| Staff (VLAN 10) | HTTP, HTTPS, DNS, ICMP (via STAFF-IN) |
| ATM (VLAN 20) | Not allowed (App Server only) |
| Guest (VLAN 30) | HTTP (80), HTTPS (443), DNS (53), ICMP (via GUEST-IN) |
| Internet (ISP) | HTTP (80), HTTPS (443), ICMP echo (via FROM-ISP) |


Without this line, `nslookup` times out even though ping works.

## 7. Testing

From a Staff PC:

```
ping 10.10.10.10
nslookup www.bank.com
```

Then open `http://www.bank.com` in the Web Browser. The Bank Ltd. home page should load.
