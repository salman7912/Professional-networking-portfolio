# MAIL Server

Email server of the Bank network (DMZ segment). Handles internal staff email for the `bank.com` domain.

## 1. Overview

| Item | Value |
|---|---|
| Device | MAIL Server |
| Location | HQ Data Center, DMZ (connected to Switch4) |
| Role | SMTP + POP3 email server |
| Domain | bank.com |

## 2. IP Configuration (Static)

| Field | Value |
|---|---|
| IPv4 Address | 10.10.10.11 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.10.1 |
| DNS Server | 10.10.10.10 |

## 3. Services

| Service | Status | Notes |
|---|---|---|
| EMAIL > SMTP | ON | Sending mail (port 25) |
| EMAIL > POP3 | ON | Receiving mail (port 110) |
| Domain Name | bank.com | Set under Services > EMAIL |

HTTP is not enabled on this server. Opening `http://mail.bank.com` in a browser gives "Request Timeout", which is expected. Mail is used through the Email client only.

## 4. DNS Record

| Name | Type | Address |
|---|---|---|
| mail.bank.com | A | 10.10.10.11 |

The record is hosted on the WEB Server (10.10.10.10).

## 5. Mail Users

| User | Email | Password |
|---|---|---|
| staff1 | staff1@bank.com | (pass123) |
| staff2 | staff2@bank.com | (pass124) |

## 6. Email Client Settings (on PC)

Desktop > Email > Configure Mail:

| Field | Value |
|---|---|
| Your Name | staff1 |
| Email Address | staff1@bank.com |
| Incoming Mail Server | mail.bank.com |
| Outgoing Mail Server | mail.bank.com |
| User Name | staff1 |
| Password | (pass123) |

| Field | Value |
|---|---|
| Your Name | staff2 |
| Email Address | staff2@bank.com |
| Incoming Mail Server | mail.bank.com |
| Outgoing Mail Server | mail.bank.com |
| User Name | staff2 |
| Password | (pass124) |

## 7. Access Control (ACL)

| Who | Access to MAIL Server |
|---|---|
| Staff (VLAN 10) | SMTP, POP3 (via STAFF-IN, 10.10.10.0/24 permitted) |
| ATM (VLAN 20) | Not allowed |
| Guest (VLAN 30) | Not allowed |
| Internet (ISP) | SMTP (25) and ICMP echo only (via FROM-ISP) |


## 8. Testing

1. On PC1 (Staff), configure the Email client as staff1.
2. On another Laptop0 (Staff) , configure the Email client as staff2.
3. From PC1: Compose, To `staff2@bank.com`, Subject `Test`, then Send.
4. On the staff2 Laptop0: click Receive. The `Test` mail should appear in the inbox.
