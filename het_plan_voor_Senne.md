# Het probleem

We draaien weliswaar een hoop servers, maar niemand gebruikt ze. Daarom zou ik graag minder stroom verbruiken zodat we kunnen verantwoorden om ze te blijven draaien.

# De oplossing

Nu hebben we

- ProxMox server 1
- ProxMox server 2
- ProxMox backup server
- Fedora NAS

Van daaruit doen we de volgende stappen:

* Stap 1:
    - ProxMox server 1
    - ProxMox server 2
    - ProxMox backup server
    - Tweede proxMox backup server
    - Fedora NAS
* Stap 2:
    - ProxMox server 1
    - ProxMox server 2
    - ~~ProxMox backup server~~ TrueNAS
    - Tweede proxMox backup server
    - Fedora NAS
* Stap 3:
    - ProxMox server 1
    - ProxMox server 2
    - ~~ProxMox backup server~~ TrueNAS
    - Tweede proxMox backup server
    - ~~Fedora NAS~~

# De tweede proxMox backup-server

Deze gaan we draaien op een optiplex die staat in het DI-lokaal. Dat klinkt dodgy (en is dat ook), maar er is wel iets voor te zeggen omdat onze backups nu off-site zijn.

Je doet daarvoor het volgende:

- Combineer de optiplex 3620 met 3x2TB en 1x250gb HDDs
- Installeer [PM Backup](https://www.proxmox.com/en/proxmox-backup-server/overview) op die computer
- Hang hem aan de switch in ons lokaal
- Configureer de switches zodat hij in VLAN 10 zit (moet alleen op de switch in het lokaal)
- Voeg hem toe aan de proxMox-cluster
- Zorg ervoor dat hij elke zaterdagmiddag om 14u opstart (in de BIOS van de PC)
- Zorg ervoor dat hij elke maandagochtend om 6u afsluit (in proxmox zelf)
- Hang met de labelmaker een plakker op de optiplex zodat iedereen weet dat deze computer niet weg mag

# TrueNas

[link](https://www.truenas.com/download-truenas-core/)

# Sidequest

We zouden de UTP-kabels die in het lokaal liggen op de switch moeten aansluiten. Stekkertjes heb ik al (liggen op de switches), een tang kan ik regelen. We zoeken nog slechts een vrijwilliger om een dagje op een stoel te balanceren om die kabels door te knippen en er nieuwe stekkers op te hangen.

Als je die zou vinden mag je dat laten horen, dan roep ik de verantwoordelijke van de school erbij zodat we alleen de juiste kabels doorknippen.