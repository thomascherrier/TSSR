# TP Samba - TSSR

## Objectifs 

Créer un partage type **MiniNas** pour TPE en respectant formalisme,nommage et bonnes pratiques.

## Contexte

- 1 VM cliente Win Pro en workgroup
- 1 VM server samba sous debian
- Dans le même réseau local

## Réseau

1 réseau à plat en /24
1 workgroup nommé Contoso
le poste client windows est en dhcp dans le réseau et se nomme poste01
Le poste serveur debian est en IP fixe et se nomme nas

### Datastore

Créer sur le serveur de fichier cette arborescence

- /partage
- /partage/direction
- /partage/compta
- /partage/communication
- /partage/commun
- /partage/public

peupler de fichier témoin cette arborescence

### Utilisateurs

- Mr Dupuis : directeur
- Jean Boulier : comptable
- Mlle Jeanne : Communication
- Gaston Lagaffe : Salarié
- Yves Lebrac : Salarié

La VM Windows sera multicompte

### Droits

- Tout le monde possède un accès R sur public
- Tout le monde possède un accès RW sur commun
- Les personnes ont toutes un accès en RW sur un dossier perso
- Dupuis a un accès RW sur direction
- Boulier a un accès RW sur compta
- Jeanne a un accès RW sur communication

### Bonus

- Monter les partage en raccourcis dans les bureaux ou l'explorateur de fichier
- Sécuriser au mieux les droits
- Faire un backup