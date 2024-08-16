# Gone

File kept for backup. Actually more for "I should delete this but I'm not like, 100% sure to delete it."

## Static IP's

| IP           | Device                 | Use                    | Notes                    |
|--------------|------------------------|------------------------|--------------------------|
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

# Hardware

| Dell Equallogic PS6110                   | SAN                              |                                    |
| Dell PowerEdge M1000E (2x M630, 8x M620) | Blade enclosure, running proxmox | M620's don't have RAID controllers |


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