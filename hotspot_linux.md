Voici plusieurs approches, de la plus simple à la plus complète.

## Méthode rapide avec `nmcli` (NetworkManager)

C'est le plus simple si NetworkManager est installé.

```bash
# Démarrer le hotspot
nmcli device wifi hotspot ifname wlan0 ssid MonHotspot password "MotDePasse123"
```

Remplace `wlan0` par ton interface Wi-Fi (vérifie avec `iw dev` ou `ip link`).

Ensuite, pour voir les appareils connectés **en temps réel** :

```bash
watch -n 2 "iw dev wlan0 station dump | grep Station"
```

`iw ... station dump` liste directement les stations (clients) associées à ton point d'accès, et `watch -n 2` rafraîchit toutes les 2 secondes.

## Affichage plus lisible (MAC + rafraîchissement)

```bash
watch -n 2 'echo "=== Appareils connectés ($(date +%H:%M:%S)) ==="; iw dev wlan0 station dump | grep Station | awk "{print \$2}"'
```

Ça n'affiche que les adresses MAC, avec l'heure de mise à jour.

## Voir aussi les IP attribuées

Si tu veux relier chaque MAC à une IP (utile pour identifier les appareils) :

```bash
watch -n 2 "ip neigh show dev wlan0"
```

Ou, si le hotspot distribue des baux DHCP via dnsmasq :

```bash
watch -n 2 "cat /var/lib/misc/dnsmasq.leases"
```

## Méthode complète avec `create_ap`

Si tu veux un outil dédié qui gère hostapd + dnsmasq automatiquement :

```bash
# Installation (Debian/Ubuntu)
sudo apt install util-linux hostapd dnsmasq
git clone https://github.com/oblique/create_ap
cd create_ap && sudo make install

# Démarrer (wlan0 = Wi-Fi, eth0 = source internet)
sudo create_ap wlan0 eth0 MonHotspot MotDePasse123
```

`create_ap` affiche déjà dans son terminal les clients qui se connectent/déconnectent en temps réel, avec leurs adresses MAC.

## Quelques points pratiques

Pour que le hotspot fonctionne, ta carte Wi-Fi doit supporter le mode AP. Vérifie avec :

```bash
iw list | grep -A 10 "Supported interface modes"
```

Cherche `* AP` dans la sortie.

Veux-tu que je te fasse un petit script qui combine le démarrage du hotspot **et** l'affichage temps réel des MAC dans une seule commande ? Dis-moi juste ton interface Wi-Fi et ta distribution.
