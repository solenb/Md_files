---
title: Live Linux Creation
---
> ### <center>Sommaire 
 </center> <a id="pres"></a>

[TOC]

# Création d'un Linux Live (méthode longue + chroot)

## Prérequis
Installation des applications requises pour créer l'environnement

```
sudo apt-get install \
    binutils \
    debootstrap \
    squashfs-tools \
    xorriso \
    grub-pc-bin \
    grub-efi-amd64-bin \
    mtools
    
mkdir $HOME/live-ubuntu-from-scratch
```
## Bootstrap

### Debootstrap
```
sudo debootstrap \
    --arch=amd64 \
    --variant=minbase \
    bionic \
    $HOME/live-ubuntu-from-scratch/chroot \
    http://us.archive.ubuntu.com/ubuntu/
```
>`Debootstrap` est un outil qui permet d'installer un système Debian de base dans le sous-répertoire d'un autre système déjà existant. Il n'a pas besoin d'un CD d'installation, juste d'un accès à un dépôt Debian.
>> soource *https://wiki.debian.org/fr/Debootstrap*
### Configuration de points de montages extrenes
```
sudo mount --bind /dev $HOME/live-ubuntu-from-scratch/chroot/devsudo mount --bind /run $HOME/live-ubuntu-from-scratch/chroot/run
```

Comme nous allons mettre à jour et installer des paquets (dont grub), ces points de montage sont nécessaires dans l'environnement chroot, afin que nous puissions terminer l'installation sans erreurs.

## Environnement `Chroot`
### Accès à l'environnement  `chroot`
```
sudo chroot $HOME/live-ubuntu-from-scratch/chroot
```
### Configuration des points de montage

```
mount none -t proc /proc
mount none -t sysfs /sys
mount none -t devpts /dev/pts
export HOME=/root
export LC_ALL=C
```
Ces points de montage sont nécessaires à l'intérieur de l'environnement `chroot`, afin d'être en mesure de terminer l'installation sans erreurs.

### Configurer un nom d'hôte
```
echo "ubuntu-project-live" > /etc/hostname
```
### Configurer `/apt/sources.list`
```
cat <<EOF > /etc/apt/sources.list
deb http://us.archive.ubuntu.com/ubuntu/ bionic main restricted universe multiverse 
deb-src http://us.archive.ubuntu.com/ubuntu/ bionic main restricted universe multiverse
deb http://us.archive.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse 
deb-src http://us.archive.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://us.archive.ubuntu.com/ubuntu/ bionic-updates main restricted universe multiverse 
deb-src http://us.archive.ubuntu.com/ubuntu/ bionic-updates main restricted universe multiverse    
EOF
```

### Mettre à jours les dêpots des paquets

```
apt update
```
### Installation de systemd
``` 
apt-get install -y systemd-sysv
```
> `Systemd` est le gestionnaire de système depuis Ubuntu 16.04 LTS Xenial. Le nom de ce programme vient de « system daemon » : le daemon du système. 
C'est une pièce maîtresse de l'architecture GNU/Linux. En effet, c'est le premier programme lancé par le noyau (il a donc le PID N°1) et il se charge de lancer tous les programmes suivants en ordre jusqu'à obtenir un système opérationnel pour l'utilisateur, selon le mode déterminé (single user, multi-user, graphique). 
> > source: *https://doc.ubuntu-fr.org/systemd*

### Configuration de machine-id et divert

```
dbus-uuidgen > /etc/machine-id
ln -fs /etc/machine-id /var/lib/dbus/machine-id
```
> Le fichier `/etc/machine-id` contient l'ID de machine unique du système local qui est défini lors de l'installation ou du démarrage. L'ID de la machine est un ID unique à 32 caractères en minuscules, hexadécimal, terminé par une nouvelle ligne. Lorsqu'il est décodé en hexadécimal, cela correspond à une valeur de 16 octets/128 bits. Cet ID peut ne pas contenir que des zéros. 
> > source *https://wiki.debian.org/MachineId*

```
dpkg-divert --local --rename --add /sbin/initctl
ln -s /bin/true /sbin/initctl
```
> `dpkg-divert` sert à créer et à mettre à jour la liste des détournements.
> Les `détournements` de fichiers sont un moyen de forcer dpkg à ne pas installer un fichier dans son emplacement, mais vers un lieu détourné.
> > source *http://manpages.ubuntu.com/manpages/trusty/fr/man8/dpkg-divert.8.html*

### Installation des paquets requis pour un `Live system` 

```
apt-get install -y \
    ubuntu-standard \
    casper \
    lupin-casper \
    discover \
    laptop-detect \
    os-prober \
    network-manager \
    resolvconf \
    net-tools \
    wireless-tools \
    wpagui \
    locales \
    linux-generic
```

### Installateur graphique

```
apt-get install -y \
    ubiquity \
    ubiquity-casper \
    ubiquity-frontend-gtk \
    ubiquity-slideshow-ubuntu \
    ubiquity-ubuntu-artwork
```
### Installation du `window manager`

```
apt-get install -y \
    plymouth-theme-ubuntu-logo \
    ubuntu-gnome-desktop \
    ubuntu-gnome-wallpapers

```

### Installation de logiciels utiles

```
apt-get install -y \
    clamav-daemon \
    terminator \
    apt-transport-https \
    curl \
    vim \
    nano \
    less \
    firefox \
    dnsutils
```

### Suppression de logiciels et de paquets inutiles  

```
apt-get purge -y \
     transmission-gtk \
     transmission-common \
     gnome-mahjongg \
     gnome-mines \
     gnome-sudoku \
     aisleriot \
     hitori
apt-get autoremove -y
```
## Reconfiguration des paquets
### Reconfiguration de la langue

```
dpkg-reconfigure locales
```

Les fenêtres suivantes sont apparues : 

![](https://i.imgur.com/dUIDT3l.png)
![](https://i.imgur.com/oUGC9Ut.png)

### Reconfiguration de resolvconf (DNS)

```
dpkg-reconfigure resolvconf
```
![](https://i.imgur.com/DHSEp3j.png)
![](https://i.imgur.com/TJkBPkJ.png)

### Configuration de network-manager

```
dpkg-reconfigure network-manager
```

## Nettoyage de l'environnement chroot
```
truncate -s 0 /etc/machine-id
```

### Suppresion des diversions
```
rm /sbin/initctl
dpkg-divert --rename --remove /sbin/initctl
```

### Nettoyage final
```
apt-get clean
rm -rf /tmp/* ~/.bash_history
umount /proc
umount /sys
umount /dev/pts
export HISTSIZE=0
exit
```
### Démontage des points d'attache

```
sudo umount $HOME/live-ubuntu-from-scratch/chroot/dev
sudo umount $HOME/live-ubuntu-from-scratch/chroot/run
```

## Création de l'image Live et remplissage

### Accéder au répertoire 

```
cd $HOME/live-ubuntu-from-scratch
```
### Création de répertoires
```
mkdir -p image/{casper,isolinux,install}
```
### Copie des images de kernels

```
sudo cp chroot/boot/vmlinuz-**-**-generic image/casper/vmlinuz
sudo cp chroot/boot/initrd.img-**-**-generic image/casper/initrd
```

### Copie de memtest86+ binaires (BIOS)
```
sudo cp chroot/boot/memtest86+.bin image/install/memtest86+
```

### Téléchargement et extraction de memtest86 binaire (UEFI)

```
wget --progress=dot https://www.memtest86.com/downloads/memtest86-usb.zip -O image/install/memtest86-usb.zip
unzip -p image/install/memtest86-usb.zip memtest86-usb.img > image/install/memtest86
rm image/install/memtest86-usb.zip
```

## Configuration du Grub
### Accéder au répertoire 

```
cd $HOME/live-ubuntu-from-scratch
```
### Création du point d'accès des fichiers Grub
```
touch image/ubuntu
```
### Création du fichier de configuration du Grub

```
cat <<EOF > image/isolinux/grub.cfg

search --set=root --file /ubuntu

insmod all_video

set default="0"
set timeout=30

menuentry "Try Ubuntu FS without installing" {
   linux /casper/vmlinuz boot=casper quiet splash ---
   initrd /casper/initrd
}

menuentry "Install Ubuntu FS" {
   linux /casper/vmlinuz boot=casper only-ubiquity quiet splash ---
   initrd /casper/initrd
}

menuentry "Check disc for defects" {
   linux /casper/vmlinuz boot=casper integrity-check quiet splash ---
   initrd /casper/initrd
}

menuentry "Test memory Memtest86+ (BIOS)" {
   linux16 /install/memtest86+
}

menuentry "Test memory Memtest86 (UEFI, long load time)" {
   insmod part_gpt
   insmod search_fs_uuid
   insmod chain
   loopback loop /install/memtest86
   chainloader (loop,gpt1)/efi/boot/BOOTX64.efi
}
EOF
```

## Création du manifeste
Dans les étapes suivantes, la création du manifeste est importante car elle nous indique quelle version de chaque paquet est installée dans la version Live et quels paquets seront supprimés ou maintenus dans la version qui sera installée (persistante sur le disque dur).

### Génération des manisfestes
```
sudo chroot chroot dpkg-query -W --showformat='${Package} ${Version}\n' | sudo tee image/casper/filesystem.manifest

sudo cp -v image/casper/filesystem.manifest image/casper/filesystem.manifest-desktop

sudo sed -i '/ubiquity/d' image/casper/filesystem.manifest-desktop

sudo sed -i '/casper/d' image/casper/filesystem.manifest-desktop

sudo sed -i '/discover/d' image/casper/filesystem.manifest-desktop

sudo sed -i '/laptop-detect/d' image/casper/filesystem.manifest-desktop

sudo sed -i '/os-prober/d' image/casper/filesystem.manifest-desktop
```

## Compression du chroot
### Accéder au répertoire 

```
cd $HOME/live-ubuntu-from-scratch
```
## Création du squashfs 
> SquashFS est un système de fichiers compressé en lecture seule sous Linux.
Il peut être utilisé sur des supports de mémoire flash, CD-ROM ou disque dur pour des systèmes de fichiers disponible en lecture seulement.
Il est notamment employé pour la création de nombreux live CD ainsi qu’en informatique embarquée, en remplacement de cramfs. 
>> source *https://fr.wikipedia.org/wiki/SquashFS*

```
solenb@solenb-VirtualBox:~/live-ubuntu-from-scratch$ 
sudo mksquashfs chroot image/casper/filesystem.squashfs
```
