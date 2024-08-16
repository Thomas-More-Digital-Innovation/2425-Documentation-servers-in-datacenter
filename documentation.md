# Networking

## VLAN's

| IP         | Name       | Use     |
|------------|------------|---------|
| 10.19. 0.0 | Default    | No IP's |
| 10.19.10.0 | Management |         |
| 10.19.20.0 | Storage    | Disabled! |
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
| 10.19.10.253 | Switch D202            | Management interface   |                          |
| 10.19.10.254 | Switch datacenter      | Management interface   |                          |


## Public network

Set using DHCP, received from Gunther. In range 172.16.

No public ports or port forwarding (yet).

# Hardware

| Type                                     | Function                         | Notes                              |
|------------------------------------------|----------------------------------|------------------------------------|
| Dell R29?0                               | OPNSense                         |                                    |
| Dell PowerEdge R620                      | ProxMox servers                  | 2x                                 |
| Dell ????                                | ProxMox backup                   |                                    |
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

Tailscale:

https://tailscale.com/kb/1097/install-opnsense



