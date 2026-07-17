# Comprendre l'erreur "File too large"

## Les bases : comment un disque stocke des fichiers

Avant de comprendre l'erreur, il faut distinguer deux couches bien séparées :

**1. Le support physique** (le "disque" au sens matériel)
- **Disque dur (HDD)** : plateaux magnétiques, données organisées en secteurs
- **Mémoire flash** (clé USB, carte SD, SSD) : cellules électroniques NAND, pas de pièces mobiles
- **Disque optique** (CD/DVD) : gravure laser

Le support physique n'a en général **aucune limite intrinsèque** sur la taille d'un fichier — il stocke juste des blocs de données bruts.

**2. Le système de fichiers (filesystem)**
C'est la couche logicielle qui organise ces blocs en "fichiers" et "dossiers" que tu peux voir et manipuler. C'est **cette couche-là** qui impose des règles et des limites, indépendamment du support physique. Les systèmes de fichiers les plus courants :

| Système de fichiers | Taille max de fichier | Usage typique |
|---|---|---|
| **FAT32** | **4 Go (4 294 967 295 octets)** | Clés USB, cartes SD, compatibilité universelle |
| exFAT | 16 exaoctets (~illimité en pratique) | Clés USB modernes, cartes SD grande capacité |
| NTFS | 16 To | Windows |
| ext4 | 16 To | Linux |

## Le diagnostic de ton erreur

Ton chemin est révélateur :
```
/run/media/mokira3d48/FD1E-B0E2/
```

Le label `FD1E-B0E2` (deux groupes de 4 caractères hexadécimaux) est **la signature typique d'un volume FAT32** sous Linux. Les partitions NTFS ou ext4 ont des UUID différents (souvent plus longs, au format `xxxx-xxxx-xxxx...`).

Donc ton fichier `road_detection.zip` dépasse très probablement **4 Go**, et le système de fichiers FAT32 de ta clé USB/carte refuse de créer un fichier plus gros que cette limite. `cp` commence à copier, remplit le fichier jusqu'à la limite autorisée par le filesystem, puis échoue : `File too large`.

## Comment vérifier

```bash
# Voir le type de système de fichiers monté
df -T /run/media/mokira3d48/FD1E-B0E2

# Ou avec lsblk
lsblk -f

# Voir la taille exacte du fichier à copier
ls -lh road_detection.zip
```

Si `df -T` t'affiche `vfat`, c'est confirmé : c'est du FAT32.

## Les solutions

**Option 1 — Reformater la clé/carte en exFAT** (le plus simple, si tu peux te permettre d'effacer les données)
```bash
sudo mkfs.exfat -n MonDisque /dev/sdX1
```
⚠️ Remplace `/dev/sdX1` par le bon périphérique (vérifie avec `lsblk` avant, pour ne pas effacer le mauvais disque).

**Option 2 — Découper le fichier en morceaux** (si tu dois garder du FAT32, par exemple pour compatibilité avec de vieux appareils)
```bash
split -b 3G road_detection.zip road_detection.zip.part_
```
Puis pour recoller ensuite :
```bash
cat road_detection.zip.part_* > road_detection.zip
```

**Option 3 — Compresser davantage** si le zip peut être réduit sous les 4 Go (par exemple avec une meilleure compression, ou en excluant des fichiers inutiles).

Si tu veux, je peux t'aider à choisir entre exFAT et une autre option selon l'usage que tu comptes faire de cette clé (compatibilité avec Windows, macOS, systèmes embarqués, etc.).
