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
| 10.19.10.61  | HP Server_1            |                        |                          |
| 10.19.10.62  | HP Server_2            |                        |                          |
| 10.19.10.63  | Deep learning          |                        |                          |
| 10.19.10.81  | ProxMox backup 1       | Webinterface           |                          |
| 10.19.10.101 | ProxMox node 1         | Webinterface           |                          |
| 10.19.10.240 | ProxMox node 1         | iDrac                  | Not set yet              |
| 10.19.10.102 | ProxMox node 2         | Webinterface           |                          |
| 10.19.10.241 | ProxMox node 2         | iDrac                  | Not set yet              |
| 10.19.10.110 | TrueNas                | Management interface   |                          |
| 10.19.10.242 | TrueNas                | iDrac                  | Not set yet              |
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
| Huawei S5720-28t                         | Switch datacenter                | On loan from SIN                   |
| HP 5700 JG894A                           | Switch datacenter                | On loan from SIN                   |
| Device X ?                               | pfSense                          | On loan from SIN                   |
| Dell PowerEdge R620                      | ProxMox servers                  | 2x                                 |
| Dell ????                                | ProxMox backup                   | On loan from SIN                   |
| Dell OptiPlex 7040                       | PC@TV                            | Username: DI, password: R1234-56   |
| SuperMicro SYS-531A-IL                   | Deep learning station            | Username en passwords in bitwarden |

## HP 5700 JG894A

* Connect using console, putty, 9600 baud. 
* [link](https://support.huawei.com/enterprise/en/doc/EDOC1000178166/3168580a/restoring-the-bootrom-password)
* Reset console password using ctrl-B -> default Bootrom password is Admin@huawei.com
* Use menu to clear console password
* Reboot

```
reset saved-configuration
system-view
```
* [link](https://www.manualslib.com/manual/1737124/Huawei-S5720i-Si-Series.html?page=11#manual)
* [link](https://support.huawei.com/enterprise/en/doc/EDOC1000178170/838c65e3/configuring-a-primary-ip-address-for-an-interface)
* [link](https://support.huawei.com/enterprise/en/doc/EDOC1100325914/381ea229/vlan-mapping-configuration-commands-)
* [link](https://support.huawei.com/enterprise/en/doc/EDOC1000178166/dbec5988/configuring-a-web-user-and-logging-in-to-the-web-system)


```
interface Vlanif 10
ip address 10.19.10.254 255.255.255.0

interface GigabitEthernet 1/0/23
port default vlan 10

interface GigabitEthernet 1/0/13
port default vlan 10

interface GigabitEthernet 1/0/15
port default vlan 60


interface GigabitEthernet 1/0/24
port link-type trunk
port trunk allow-pass vlan 10 20 30 40 60

aaa
local-user jm password irreversible-cipher *%W!#wD2rXh0pDWmE
local-user jm service-type http <- Not working!!!!
local-user jm service-type ssh
local-user jm privilege level 15

local-user jm2 password irreversible-cipher *%W!#wD2rXh0pDWmE
local-user jm2 service-type http
local-user jm2 service-type ssh
local-user jm2 privilege level 15

user-interface vty 0 4
 authentication-mode aaa
 protocol inbound ssh

rsa local-key-pair create

```

* [link](https://support.huawei.com/enterprise/en/doc/EDOC1000178168/7abaffd5/link-aggregation-configuration)

```
interface eth-trunk 1
port trunk allow-pass vlan 10 20 30 40 60

interface GigabitEthernet 1/0/1
eth-trunk 1

interface GigabitEthernet 1/0/3
eth-trunk 1

interface GigabitEthernet 1/0/5
eth-trunk 1

interface GigabitEthernet 1/0/7
eth-trunk 1

interface eth-trunk 2
port trunk allow-pass vlan 10 20 30 40 60
interface GigabitEthernet 1/0/2
eth-trunk 2

interface GigabitEthernet 1/0/4
eth-trunk 2

interface GigabitEthernet 1/0/6
eth-trunk 2

interface GigabitEthernet 1/0/8
eth-trunk 2
```

```
stelnet server enable

system-view voor advanced mode, geen passwd.

SSH connection string:

```
ssh -oKexAlgorithms=+diffie-hellman-group-exchange-sha1 jm@10.19.10.254

  -oCiphers=+aes128-cbc   -oHostKeyAlgorithms=+ssh-rsa   -oPubkeyAcceptedAlgorithms=+ssh-rsa   -oMACs=+hmac-sha1,hmac-md5   
```

'Advanced' view:

```
system-view
```

# Services

## VM's

* fed-server (10.19.30.202): JM uses this to test stuff.
* fed-access (10.19.30.204): Tailscale subnet forwarder
* fed-dockerhost (10.19.30.205): Runs all docker containers

## Docker containers

* Portainer (for easy management):
    * Ports: 8000, 9443
    * [setup link](https://docs.portainer.io/start/install-ce/server/docker/linux)
    * docker run -d -p 8000:8000 -p 9443:9443 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.4
* PiHole:
    * Ports: 8080, 53, 67
    * [setup link](https://pimylifeup.com/pi-hole-docker/)
    * [other setup link](https://github.com/pi-hole/docker-pi-hole)
    * [stop stub listener port 53](https://fedoraproject.org/wiki/Changes/systemd-resolved)
* Static webhosting:
    * Ports: 80
    * ngninx with local, static html
* NextCloud (for passwords):
    * Ports: 8081
    [setup link](https://github.com/nextcloud/)all-in-one#nextcloud-all-in-one
* passbolt
    * Ports: 8002, 4433
    * [setup link](https://www.passbolt.com/ce/docker)
    * 
* Bitwarden
    * Ports: 8001
    * [setup link](https://bitwarden.com/help/install-on-premise-linux/)
    * [other setup link]()
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

