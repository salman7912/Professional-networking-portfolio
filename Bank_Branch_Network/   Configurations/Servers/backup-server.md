# Backup Server

Backup server of the Bank network (Server Farm segment). It stores backup files and router/switch configuration backups using FTP and TFTP.

## 1. Overview

| Item | Value |
|---|---|
| Device | Backup Server |
| Location | HQ Data Center, Server Farm (connected to Switch5) |
| Role | File backup (FTP) and device config backup (TFTP) |
| Subnet | 10.10.30.0/24 |

## 2. IP Configuration (Static)

| Field | Value |
|---|---|
| IPv4 Address | 10.10.30.12 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.30.1 |
| DNS Server | 10.10.10.10 |

## 3. Services

| Service | Status | Notes |
|---|---|---|
| FTP | ON | User: backup,password:backup123  permissions: Write, Read, Delete, Rename, List |
| TFTP | ON | Used for `copy running-config tftp` |


## 4. Access Control (ACL)

| Who | Access to Backup Server |
|---|---|
| Staff (VLAN 10) | Allowed |
| ATM (VLAN 20) | Allowed |
| Guest (VLAN 30) | Blocked |
| App Server (10.10.30.11) | Allowed (same subnet) |
| Routers and switches | Allowed for TFTP (see below) |

## 5. FTP Usage

From the App Server (Desktop > Command Prompt):

```
ftp 10.10.30.12
Username: backup
Password: backup123
```

Commands that work at the `ftp>` prompt:

| Command | Action |
|---|---|
| `dir` | List files on the server |
| `put filename` | Upload a file |
| `get filename` | Download a file |
| `delete filename` | Delete a file |
| `rename old new` | Rename a file |
| `quit` | Exit FTP |

Router commands such as `en` or `copy running-config tftp` do not work at the `ftp>` prompt.

## 6. Router Config Backup (TFTP)

Run on the router CLI, not inside FTP:

```
Core-Router1# copy running-config tftp
Address or name of remote host []? 10.10.30.12
Destination filename [Core-Router1-confg]? core1-backup
```

`!!` in the output means the backup succeeded. Check the file under Backup Server > Services > TFTP.

Suggested backup filenames:

| Device | File |
|---|---|
| Core-Router1 | core1-backup |
| Core-Router2 | core2-backup |
| CTG-BR | ctg-br-backup |
| SYL-BR | syl-br-backup |
| RAJ-BR | raj-br-backup |

## 7. Testing

From the App Server:

```
ping 10.10.30.12
ftp 10.10.30.12
```

Expected result: ping replies and FTP login succeeds (`230 Logged in`).

From a Staff PC, `ping 10.10.30.12` should fail. This is correct.
