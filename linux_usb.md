# Monter un USB sous Linux
Voici la procédure standard pour monter une clé USB depuis le terminal.

**1. Identifier la clé**

Branche la clé, puis liste les périphériques de bloc :

```bash
lsblk
```

Tu verras quelque chose comme :

```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 238.5G  0 disk
├─sda1   8:1    0   512M  0 part /boot/efi
└─sda2   8:2    0   238G  0 part /
sdb      8:16   1  14.6G  0 disk
└─sdb1   8:17   1  14.6G  0 part
```

Ta clé est typiquement le disque avec `RM` (removable) à `1` et une taille qui correspond. Ici c'est `sdb`, et la **partition** à monter est `sdb1`. Tu peux aussi utiliser :

```bash
sudo fdisk -l
```

pour voir le type de système de fichiers.

**2. Créer un point de montage**

```bash
sudo mkdir -p /mnt/usb
```

**3. Monter la partition** (et non le disque entier — donc `sdb1`, pas `sdb`)

```bash
sudo mount /dev/sdb1 /mnt/usb
```

En général le système de fichiers est détecté automatiquement. Si ce n'est pas le cas, précise-le avec `-t` (par exemple `-t vfat` pour FAT32, `-t exfat`, `-t ntfs`).

**4. Accéder aux fichiers**

```bash
ls /mnt/usb
cd /mnt/usb
```

**5. Démonter avant de retirer la clé** (important pour éviter la corruption des données)

```bash
sudo umount /mnt/usb
```

Quelques cas particuliers : pour une clé en **exFAT**, il faut parfois installer `exfat-fuse` / `exfatprogs`. Pour **NTFS**, c'est `ntfs-3g`. Et pour pouvoir écrire en tant qu'utilisateur normal sur du FAT/exFAT, tu peux ajouter les options de montage :

```bash
sudo mount -o uid=$(id -u),gid=$(id -g) /dev/sdb1 /mnt/usb
```

Si la commande `mount` te renvoie une erreur, montre-moi le message exact.
