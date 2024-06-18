# Networking

## VLAN's

| IP | Name |
|--- |---     |
 10.19.0.0|Default|
 10.19.10.0|Management|
 10.19.20.0|Storage|
 10.19.30.0|Services|
 10.19.40.0|IoT|
 10.19.60.0|Clients|

## Static IP's

| IP | Device | Use |
|--- |---     |---  |
|10.19.X.1 | OPNsense| Default gateway vlan X |
|10.19.10.254 | Switch datacenter| Management interface |
|10.19.10.253 | Switch D202| Management interface |

## Public network

Set using DHCP, received from Gunther. In range 172.16.

No public or port forwarding (yet).

# ProxMox

## Blade enclosure


## Servers


## ProxMox backup


# SAN (Dell Equallogic)


# D202

## Camera

# VM's

- OpenVPN?
- Home Assistant
- BitWarden
- PiHole
- Dockerhost?