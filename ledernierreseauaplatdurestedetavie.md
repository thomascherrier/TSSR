# TP TSSR25_1 : Le dernier réseau à plat du reste de ta vie...

## Préambule
- Choisir un nom d'entité, un contexte rapide et un logo.
- Créer un schéma réseau de l'infrastructure sous draw.io
tous les équipements sont pinguables
- Votre plan d'adressage doit être rationnel et les équipements fixe seront en fin de réseau

Les clients (PC,Portables,mobiles,tablettes...) seront en DHCP

## Attribution dynamique des adresses
- Votre serveur DHPCP doit pousser les élements suivant
- L'adresse de la gtw
- Le masque
- Le serveur DNS Préféré (votre serveur en local)
- Le serveur de temps **ntp.unice.fr**
- La plage DHCP doit être en début (genre 01-30)
- La durée du bail est de 24h



## Plan de nommage et DNS
- Le rôle DNS est installé sur un windows serveur
- Il  n'y a pas de rôle active directory
- Une zone reverse doit être crée
- Les roots servers sont mis à jour
- Les nom FQDN doivent être qualifiée
- Créer des records A pour chaque équipements
- Vous pouvez créer des CNAME

## Equipement
- 1 Routeur SOHO qui possède une IP WAN fixe

Ce routeur assigne un LAN en **classe B/26**.
Ce routeur est une gtw nat donc vous gérerez la conf et la sécurité via une interface web

- 1 commutateur de Niveau 2 
Ce switch sera configuré en IP Fixe et sera pleinement exploité au niveau des ses paramêtres - sauf les VLANs

- 1 AP sera configuré en IP Fixe et sera pleinement exploité au niveau des ses paramêtres et sera accessible avec un SSID et une PSK avec les configurations les plus sécurisées possibles
(un switch POE passif est peut-être requis)

- 1 NAS a configurer qui fera les services SMBs (partage de fichier)

- 1 Imprimante en réseau

- 1 server Windows 2022 avec le rôle DHCP (accès RDP ouvert, FW sur réseau privé, MAJ coupées)

- 1 server Windows 2022 avec le DNS (accès RDP ouvert, FW sur réseau privé, MAJ coupées)

## clients
- Des Vms client win10 ou win11 peuvent être créer sous VMWare Workstation sur vos PC respectifs
- Vos postes fixes sont également en wifi
- Ces postes sont dans un workgroup dont le nom est lié au domaine (ie: bobdy)

## Exemple de nomenclature pour le domaine bobdy.lan

- 172.16.1.62 gtw.bobdy.lan
- 172.16.1.61 switch.bobdy.lan
- 172.16.1.60 ap.bobdy.lan
- 172.16.1.59 share.bobdy.lan
- 172.16.1.58 printer.bobdy.lan
- 172.16.1.57 ns.bobdy.lan
- 172.16.1.56 dhcp.bobdy.lan
- ...

### NB
tous les accès aux équipements se feront via un FQDN et si possible uniquement via des protocoles chiffrés
Si il y a http, il faut veiller si possible à rediriger vers https ou couper http
Les FW sont activés sur les équipemens en possédant
Vous utiliserez des certficats auto-signé 
Vous choisiez une stratégie de mots passe robuste sur tous les équipements

### Bonus

Installer un serveur web sur un des équipement ou serveur pour arriver sur une simple page avec une URL du type https://www.bobdy.lan