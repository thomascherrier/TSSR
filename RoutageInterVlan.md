# TP : Routage InterVlan

### Préambule

choisir un nom de domaine qui se substituera à *mondomaine.lan* ci-dessous

## Configuration du Routeur PFSense

Assigner une ip fixe en WAN sur l'interface RE0 du PFSense selon la nomenclature ci-dessous :

Upstream gateway **188.231.29.3/27**
Netmask 255.255.255.192

- groupe 1 188.231.29.21
- groupe 2 188.231.29.22
- groupe 3 188.231.29.23
- groupe 4 188.231.29.24

Du côté LAN du routeur/gtw :
- 192.168.X.254/24
- X est le numéro de votre groupe

Ce rezo ne sera quasiment pas utilisé du fait de la déclaration des VLANs et du routage intervlan.

Votre routeur PFSense fera les rôles suivant
- FW
- NAT
- DHCP temporaire
- DNS temporaire
- Le rôle OpenVPN Server sera configuré a terme pour recevoir des connexions Client2Site

### Conseils :
- Créer une VLAN Database (10 SRV,20 Client,30 Wifi,40 Virt,99 admin) en concordance avec ce qui va être déclarer sur le switch Cisco
- Créer des SubGTW en .254 pour chaque sous-réseau
- Déclarer une règle passoire any dans chaque zone du FW pour les VLANs
- Installer les paquets suivant sur le PFSense : OpenVPNClientExport, FTP Proxy, Nmap
- Le lien trunk du câble ethernet qui arrive sur le PFSense sera bien déclaré dans le switch
- Ne pas oublier à terme de déclarer le service DHCP Relay quand votre serveur windows avec le rôle DHCP sera déclaré
- Vous devrez accéder à ce switch sur https://gtw.mondomaine.lan:666


*Bonus :* 
- Configurer un captive portal sur le VLAN WIFI
- Configurer un serveur OpenVPN Site2Site

## Switch 1 :
Un cisco catalyst 2960 ou 3950 sera utilisé comme swith maitre.

il aura comme ip 192.168.99.253 dans le VLAN Admin pour activer l'interface web dessus en 443 si possible.


Ce switch est le switch maitre pour le VTP
Vous déclarerez vos VLAN dedans  :
- VLAN 10 SRV
- VLAN 20 Client
- VLAN 30 Wifi
- VLAN 40 Virt
- VLAN 99 Admin

Déclarez vos appartenance de ports (range) dans votre switch en fonction du nombre total de port

Le dernier port 24 (ou 48) sera le port trunk <--> gtw
l'avant dernier port 23 (ou 47) sera le port trunk <--> switch 2

## Switch 2 :
Un cisco catalyst 2960 ou 3950 sera utilisé comme switch slave via le protocol VTP.

Il aura comme ip 192.168.99.252 dans le VLAN Admin pour activer l'interface web dessus en 443 si possible

La VLAN Database (sh vlan doit remonter les vlans automatiquement une fois le VTP configuré)

Déclarez vos appartenance de ports (range) dans votre switch en fonction du nombre total de port

le dernier port 24 (ou 48) sera le port trunk <--> switch 1

*Bonus :*
Activer une LACP(LAGG) entre les 2 switch et aussi donc le protocole STP pour éviter les tempêtes de broadcast

## WIFI

Une borne wifi en https://ap.mondomaine.lan et en ip fixe 192.168.30.253 doit être configuré pour diffuser un SSID pertinent en 5Ghz avec une passphrase pour du WPA2

Vos clients mobiles doivent recevoir un bail DHCP en 192.168.30.X grâce à cette AP qui va forwarder la demande au PFSENSE qui grâce au DHCP relay va pousser cette demande au serveur windows DHCP dans le vlan 10

*Bonus :* 
Enlever la passphrase si vous êtes en portail captif


## DNS

un serveur Windows ns1.mondomaine.lan en ip fixe 192.168.10.253 doit gérer la zone mondomaine.lan

- Actualisez les roots dns dans ce serveur
- Ajouter le redirecteur 1.1.1.1
- Ajouter 4 zones reverse pour 10,20,30,40,99
- Créer des record A pour toutes vos ressources voire des CNAME

## DHCP
un serveur Windows dhcp.mondomaine.lan en ip fixe 192.168.10.252 doit gérer la diffusion du DHCP multipool pour les VLAN 20,30 (voire 40 & 99)

Le serveur doit diffuser :
- la GTW
- le netmask
- le serveur DNS
- le serveur de Temps
- un bail de 12h

## Virtualisation
Un serveur ESXI 8 ou Proxmox doit être installé en ip fixe sur 192.168.40.253 ( FQDN : esx.mondomaine.lan ou pve.mondomaine.lan)

Cette solution de virtualisation doit hébgerger une VM Windows ou Linux qui héberge un serveur web **www.
mondomaine.lan** ( 192.168.10.251) qui doit être taggé dans le VLAN 10 bien qu'étant branchée sur un ordinateur dans le VLAN 40

## Client en DHCP

Vos postes physiques (ou virtualisés) Win 10/11 doivent recevoir un bail DHCP en 192.168.20.X grâce à leur appartenance filaire au VLAN 20 qui va forwarder la demande au PFSENSE via le trunk qui grâce au DHCP relay va pousser cette demande au serveur windows DHCP dans le vlan 10...

**Have Fun**


