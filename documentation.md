# Networking

## VLAN's

| IP         | Name       | Use     |
|------------|------------|---------|
| 10.19. 0.0 | Default    | No IP's |
| 10.19.10.0 | Management |         |
| 10.19.20.0 | Storage    |         |
| 10.19.30.0 | Services   |         |
| 10.19.40.0 | IoT        |         |
| 10.19.60.0 | Clients    |         |

## Static IP's

| IP           | Device                 | Use                    | Notes                    |
|--------------|------------------------|------------------------|--------------------------|
| 10.19.X.1    | OPNsense               | Default gateway vlan X | For now only in subnet 0 |
| 10.19.10.81  | ProxMox backup 1       | Webinterface           |                          |
| 10.19.10.XXX | ProxMox backup 1       | iDrac                  | Not set yet              |
| 10.19.10.101 | ProxMox node 1         | Webinterface           |                          |
| 10.19.10.XXX | ProxMox node 1         | iDrac                  | Not set yet              |
| 10.19.10.102 | ProxMox node 2         | Webinterface           |                          |
| 10.19.10.XXX | ProxMox node 2         | iDrac                  | Not set yet              |
| 10.19.10.103 | ProxMox node 3/Blade 1 | Webinterface           |                          |
| 10.19.10.XXX | ProxMox node 3         | iDrac                  | Not set yet              |
| 10.19.10.104 | ProxMox node 4/Blade 2 | Webinterface           | Theoretically online     |
| 10.19.10.XXX | ProxMox node 4         | iDrac                  | Not set yet              |
| 10.19.10.105 | ProxMox node 5         | Webinterface           | Theoretically online     |
| 10.19.10.XXX | ProxMox node 5         | iDrac                  | Not set yet              |
| 10.19.10.106 | ProxMox node 6         | Webinterface           | Theoretically online     |
| 10.19.10.XXX | ProxMox node 6         | iDrac                  | Not set yet              |
| 10.19.10.107 | ProxMox node 7         | Webinterface           | Theoretically online     |
| 10.19.10.XXX | ProxMox node 7         | iDrac                  | Not set yet              |
| 10.19.10.108 | ProxMox node 8         | Webinterface           | Theoretically online     |
| 10.19.10.XXX | ProxMox node 8         | iDrac                  | Not set yet              |
| 10.19.10.253 | Switch D202            | Management interface   |                          |
| 10.19.10.254 | Switch datacenter      | Management interface   |                          |


## Public network

Set using DHCP, received from Gunther. In range 172.16.

No public or port forwarding (yet).

# Hardware


| Type                                     | Function                         | Notes                              |
|------------------------------------------|----------------------------------|------------------------------------|
| Dell R29?0                               | OPNSense                         |                                    |
| Dell PowerEdge R620                      | ProxMox servers                  | 2x                                 |
| Dell ????                                | ProxMox backup                   |                                    |
| Dell Equallogic PS6110                   | SAN                              |                                    |
| Dell PowerEdge M1000E (2x M630, 8x M620) | Blade enclosure, running proxmox | M620's don't have RAID controllers |
| HP 5700 JG894A                           | Switch datacenter                |                                    |
| TP-Link TL-SG3424                        | Switch D202                      |                                    |
| Dell OptiPlex 7040                       | PC@TV                            | Username: DI, password: R1234-56   |
| SuperMicro SYS-531A-IL                   | Deep learning station            | Username en passwords in bitwarden |

# VM's

- OpenVPN?
- Home Assistant
- BitWarden
- PiHole
- Dockerhost?

# Configuration

## OPNSense

Created vlans (https://homenetworkguy.com/how-to/configure-vlans-opnsense/).

DHCP settings:
* Range: 10.19.X.1 - 10.19.X.254
* Default gateway: 10.19.X.1
* DNS-server: 10.19.0.1
* Domain name: digitalinnovation.be

Specials:

* No DHCP: 10, 20
* Delay: 30_5s, 40_6s, 0_20s

Tailscale:

https://tailscale.com/kb/1097/install-opnsense

## Dell Equallogic SAN

> A null modem DB9 cable is all you need to get serial access to one of these, a pretty standard piece of IT kit. Default/standard putty settings with baud rate 9600 and you’ll be in. default login/pw is grpadmin/grpadmin. Once you log in, you’ll either see that the unit is not configured yet (indicated by a prompt), or it will just drop you to a shell where you can run commands. To see current IP info there’s a show command, but I can’t remember the exact syntax off the top of my head. The help menus in there are pretty good though.


login/pw is grpadmin/grpadmin
Membername: DINAS
Network interface: eth0
IP for net int: 10.19.10.71 /24
Default gateway: 10.19.10.1
name of the group the array will join: dinasgrp
group ip address: 10.19.10.70


Password managing group membership: R1234.56
password grpadmin-account: R1234.56?

volume:
* chap account name: disk_man
* iscsi initiator name: iscsi-init

