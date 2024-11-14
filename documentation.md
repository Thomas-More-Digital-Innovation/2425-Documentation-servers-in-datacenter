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
| 10.19.X.1    | pfSsense               | Default gateway vlan X | For now only in subnet 0 |
| 10.19.10.71  | Fedora Nas 1           | Webinterface           |                          |
| 10.19.10.XXX | Fedora Nas 1           | iDrac                  | Not set yet              |
| 10.19.10.81  | ProxMox backup 1       | Webinterface           |                          |
| 10.19.10.XXX | ProxMox backup 1       | iDrac                  | Not set yet              |
| 10.19.10.101 | ProxMox node 1         | Webinterface           |                          |
| 10.19.10.XXX | ProxMox node 1         | iDrac                  | Not set yet              |
| 10.19.10.102 | ProxMox node 2         | Webinterface           |                          |
| 10.19.10.XXX | ProxMox node 2         | iDrac                  | Not set yet              |
| 10.19.10.252 | Switch 1 D202          | Management interface   |                          |
| 10.19.10.253 | Switch 2 D202          | Management interface   |                          |
| 10.19.10.254 | Switch datacenter      | Management interface   |                          |
| 10.19.60.4   | Accesspoint DI-lokaal  | Management interface   |                          |


## Public network

Set using DHCP, received from Gunther. In range 172.16.

No public ports or port forwarding (yet).

## Wifi

Configured using accesspoint in D202.

Dino wireless / ?
Dino IoT / d1n0

# Hardware

| Type                                     | Function                         | Notes                              |
|------------------------------------------|----------------------------------|------------------------------------|
| HP 5700 JG894A                           | Switch datacenter                | On loan from SIN                   |
| Device X ?                               | pfSense                          | On loan from SIN                   |
| Dell PowerEdge R620                      | ProxMox servers                  | 2x                                 |
| Dell ????                                | ProxMox backup                   | On loan from SIN                   |
| Dell R2950                               | Fedora Nas                       |                                    |
| Dell OptiPlex 7040                       | PC@TV                            | Username: DI, password: R1234-56   |
| SuperMicro SYS-531A-IL                   | Deep learning station            | Username en passwords in bitwarden |

# Services

## VM's

* fed-server (10.19.30.202): JM uses this to test stuff.
* fed-access (10.19.30.204): Tailscale subnet forwarder
* fed-dockerhost (10.19.30.205): Runs all docker containers

## Docker containers

* Portainer: For easy management
    * Ports: 8000, 9443
    * [setup link](https://docs.portainer.io/start/install-ce/server/docker/linux)
    * docker run -d -p 8000:8000 -p 9443:9443 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.4
* PiHole:
    * Ports: 8080, 53, 67
    * [setup link](https://pimylifeup.com/pi-hole-docker/)
    * [other setup link](https://github.com/pi-hole/docker-pi-hole)
    * [stop stub listener port 53](https://fedoraproject.org/wiki/Changes/systemd-resolved)
* passbolt
    * Ports: 8002, 4433
    * [setup link](https://www.passbolt.com/ce/docker)
    * 
* Bitwarden
    * Ports: 8001
    * [setup link](https://bitwarden.com/help/install-on-premise-linux/)
    * [other setup link]()
* Static webhosting:
    * Ports: 80
    * ngninx with local, static html
* KeeWeb:
    * Ports: 8003, 4434
    * [setup link](https://github.com/keeweb/keeweb)

# Configuration

## pfSense

DHCP settings:
* Range: 10.19.X.10 - 10.19.X.254
    * 10.19.30.10 - 10.19.30.200, static mapping after that
* Default gateway: 10.19.X.1
* DNS-server: 10.19.0.1
* Domain name: digitalinnovation.be

## Fedora NAS

User: di_admin
Password: See bitwarden

Fedora server 40. Link aggregation for enp5s0 and enp9s0 to vlan 10. enp11s0 is in vlan 60, but subject to change.

[NFS-share setup](https://docs.stg.fedoraproject.org/en-US/fedora-server/services/filesharing-nfs-installation/)

```
sudo adduser -c 'nfs pseudo user' -M  -r  -s /sbin/nologin  nfs --> did not work, only works with a lot of chown -R 777 nfs

sudo mkdir -p /srv/nfs/{iso,vms}
sudo chown  -R  nfs.nfs  /srv/nfs/*

sudo firewall-cmd --permanent --add-service=nfs
sudo firewall-cmd --reload

```
[firewall](https://www.baeldung.com/linux/firewalld-nfs-connections-settings), starting from "3.3. Allowing Supplementary Services"

```
firewall-cmd --permanent --add-service=rpc-bind --add-service=mountd
firewall-cmd --permanent --add-port=32767/tcp --add-port=32767/udp --add-port=32765/tcp --add-port=32765/udp
```


