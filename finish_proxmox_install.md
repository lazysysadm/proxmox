### 1 : AVANT DE POUVOIR CREER DES VM SUPPRIMER /dev/pve/data pour ne garder que 1 volume
```shell
lvremove /dev/pve/data
```
### 2 : RESIZE DE L'ESPACE
```shell
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```
### 3: DESACTIVER LA MISE EN VEILLE PROLONGEE DU SERVER SI PROXMOX EST INSTALLE SUR UN PC PORTABLE
```shell
systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

### 4 : ETTEINDRE ECRAN AUTOMATIQUEMENT SI PROXMOX EST INSTALLE SUR UN PC PORTABLE
Dans le fichier **/etc/default/grub** ajouter/completer cette ligne : 
```shell
GRUB_CMDLINE_LINUX="consoleblank=300"
```
Puis executer la commande :
```shell
update-grub
```
### 5 :DESACTIVER LE MESSAGE DE SUBSCIPTION PROXMOX PRO :
```shell
sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service
```
### 6: Dans Datacenter > editer Directory > Content choisir : Disk Image, ISO image, Container , Container Templates

### 7 : Creer une VM :
En haut à droite en bleu cliquer sur **CREATE VM** 

**General** : Donner un nom à la VM

**OS**      : Choisir source d'installation : ISO / CD ...

**System** : Laisser par default

**Hard Disk** : CHoisir la taille du HDD

**CPU** : Choisir le nbr de cores

**Memory** : Taille memoire allouée à la VM

**Network** : Configuration reseau VM

**Confirm** : Recapitulatif avant confirmation de la creation de la machine

### 8 : Creer un container LXC 
Cliquer sur **Create CT**

**General** : Donner un nom hostname + Mot de Passe (longueur 8 ou +)

**OS** : choisir le template qu'on a prealablement telechargé avant

**Root Disk** : taille du disque du container LXC

**CPU** : Nombre de cores

**memory** : taille de la memoire du Container LXC

**network** : Paramatrages réseaux du container LXC

**DNS** : Parametrage DNS : 1.1.1.1 8.8.8.8

**Note** : Pour avoir les Container Templates se rendre dans **local > CT Templates > Templates** et télecharger le template de container LXC voulu

### Update les CT Templates :
```shell
pveam update
```
### Editer les sources listes pour ne pointer que sur la version communautaire :

```shell
/etc/apt/sources.list

deb http://ftp.fr.debian.org/debian bullseye main contrib

deb http://ftp.fr.debian.org/debian bullseye-updates main contrib

#security updates
deb http://security.debian.org bullseye-security main contrib

#Community MAJ sans souscription
deb http://download.proxmox.com/debian bullseye pve-no-subscription
```
Commenter le contenu du fichier **/etc/apt/sources.list.d/pve-enterprise.list**

```shell
#deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
```
