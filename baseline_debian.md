# Baseline CLI Linux Debian

## CLI post install

### Mise à jour

` apt update && apt upgrade -y `

### Installation des BinUtils

` apt install ssh zip nmap locate ncdu curl git screen dnsutils net-tools sudo lynx `

- ssh : installation du serveur ssh
- zip : (et unzip inclus) pour gérer les archives zip
- nmap : pour scanner les services actifs
- locate : pour trouver des fichiers indexés sur notre machine (faire la commande `updatedb` également)
- ncdu : permet de voir l'usage disque de manière simplifié (NCurses Disk Usage)
- curl : client http indispensable
- git : client git pour cloner des projet
- screen : terminal virtuel
- dnsutils : permet de disposer de la commande `dig` entre autre
- net-tools : permet de disposer de l'ancienne commande  `ifconfig`
- sudo 
- lynx : navigateur http(s) en CLI

### installation de la couche **netbios** (uniquement pour les postes en local non exposé sur Internet)

`apt install winbind samba`

modifer le fichier **nsswitch** et ajouter wins à la fin de la ligne **hosts**

`nano /etc/nsswitch.conf`

### Personnalisation du BASH

`nano /root/.bashrc`
 
*décommenter les lignes 9 à 13 incluses*


### Configuration du Réseau

configuration des IF (interfaces)
`nano /etc/network/interfaces`

mettre à minima pour un passage en IP Fixe

    auto ens33
    iface ens33 inet static
        address 192.168.X.Y/24
        gateway 192.168.X.Z


configuration du DNS `nano /etc/resolv.conf`
changer également votre search

`nano /etc/resolv.conf`

configuration du **hostname**
`nano /etc/hostname`

afficher l'IP :
`ip a`

### Installation de WebMin
cf https://webmin.com/download/

`curl -o webmin-setup-repo.sh https://raw.githubusercontent.com/webmin/webmin/master/webmin-setup-repo.sh`

`sh webmin-setup-repo.sh`

`apt install webmin --install-recommends`

Pour lancer webmin sur un autre poste :
https://votreipouFQDN:10000
### Bonus Fun

`apt install bsdgames`

Comment accéder et lancer les jeux :-) ?

`cd /usr/games`

Pour lancer un jeu

`./nomdujeu`

