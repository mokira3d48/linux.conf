# Linux.conf
Several commands suggested to install some programs on Unix system.

## Install PHP 8.2

```sh
sudo add-apt-repository ppa:ondrej/php; \
sudo apt update; \
sudo apt install php8.2 -y
```

You can verify version of your PHP with following command :

```sh
php --version
```

```sh
sudo apt install php-xml
```


```sh
sudo apt update;\
sudo apt install libreoffice -y;\
sudo apt install kolourpaint;\
sudo apt install snapd -y;\
sudo apt install nedit -y;\
sudo apt install micro -y;\
sudo apt install mc -y;\
sudo apt install xz-utils -y;\
sudo apt install python-tk -y;\
sudo apt install -y remmina;\
sudo apt install remmina-plugin-rdp -y;\
sudo apt install tor -y;\
sudo apt install tree -y;\
sudo apt install npm -y;\
sudo apt install cmatrix -y;\
sudo apt install htop -y;\
sudo apt install -y cmake;\
sudo apt install -y telegram-desktop;\
sudo apt install -y kwrite python3-pip python3-venv python3-tk python3-dev libsqlite3-dev;\
sudo apt install -y aptitude terminator;\
sudo apt install -y youtubedl-gui idle;\
sudo apt install -y code-oss postgresql php-pgsql postgresql-client postgresql-contrib libpq-dev openjdk-17-jdk
sudo apt install telegram-desktop;\
sudo apt install wget gpg autossh;\
sudo apt-get install python3-pyaudio portaudio19-dev; \
sudo apt install phpunit php-cli php-mbstring unzip php-mysql;
sudo apt-get install fonts-dejavu fonts-liberation fonts-noto;


```

## Latex install
- On Ubuntu :

```sh
sudo apt-get install texlive-base texlive-latex-recommended texlive-lang-french texlive-lang-english texlive-science texlive-fonts-recommended texlive-font-utils texlive-latex-recommended-doc texlive-science-doc texlive-binaries texlive-pictures texlive-pictures-doc texlive-metapost texlive-metapost-doc texlive-fonts-extra-doc texlive-fonts-extra-links texlive-bibtex-extra texlive-formats-extra texlive-latex-base texlive-latex-base-doc texlive-publishers texlive-publishers-doc latexmk
```

- On kali linux

```sh
sudo apt install texlive-full
```

- For the twice

```sh
sudo apt-get install pandoc texlive-latex-base texlive-fonts-recommended texlive-extra-utils texlive-latex-extra -y
```
```sh
sudo apt install -y texmaker
```

OR

```sh
sudo apt install -y texstudio
```

## Install colorschema in micro

```sh
micro -plugin install nordcolors
```

## Installation of snap apps
```sh
sudo systemctl start snapd.service;\
sudo snap install heroku --classic
```
## Node js
```sh
sudo curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo bash -;\
sudo apt-get install -y nodejs

```

## Grip

```sh
# ~$
pip3 install -U grip
```

## VScode on kali linux

```sh
sudo apt-get install -y wget gpg;\
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg;\
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg;\
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list';\
rm -f packages.microsoft.gpg;\
sudo apt install apt-transport-https -y;\
sudo apt update
```

```sh
sudo apt install code # or code-insiders
```

## Signal messenger

```sh
# NOTE: These instructions only work for 64 bit Debian-based
# Linux distributions such as Ubuntu, Mint etc.

# 1. Install our official public software signing key
wget -O- https://updates.signal.org/desktop/apt/keys.asc | gpg --dearmor > signal-desktop-keyring.gpg;\
cat signal-desktop-keyring.gpg | sudo tee /usr/share/keyrings/signal-desktop-keyring.gpg > /dev/null;\

# 2. Add our repository to your list of repositories
echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/signal-desktop-keyring.gpg] https://updates.signal.org/desktop/apt xenial main' |\
  sudo tee /etc/apt/sources.list.d/signal-xenial.list;\

# 3. Update your package database and install signal
sudo apt update && sudo apt install signal-desktop
```

## Install docker on kali linux

```sh
sudo apt install docker.io
```

```sh
sudo groupadd docker;\
sudo usermod -aG docker $USER;\
newgrp docker;\
docker ps  # To show list of conteners.
```

## Install bluetooth on kali linux 2022

```sh
sudo apt-get update;\
sudo apt-get install blueman bluez bluetooth;\
sudo systemctl enable bluetooth.service;\
sudo systemctl start bluetooth.service
```

## Install python3.9

```sh
# ...

./configure --enable-loadable-sqlite-extensions && make && sudo make install
```

```
F41TC0MM3CH3ZT012023*
guestepitech
Renov2023EPITECH
```

## install c++ tool for web server

```sh
sudo apt update; \
sudo apt -y install libmicrohttpd-dev
```

## To install protonvpn
[ProtonVPN](https://protonvpn.com/support/official-linux-vpn-kali/)

## Installing of Composer

```sh
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
sudo php composer-setup.php --install-dir=/bin --filename=composer && \
php -r "unlink('composer-setup.php');"

```

## Installing of symfony

```sh
curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash && \
sudo apt install symfony-cli

```

## Setting date and time on Kali linux

## 🎯 Définition de "efficace" pour la gestion du temps :

### **Efficacité à court terme** :
- Résultat immédiat et visible
- Simple à exécuter
- Pas besoin de configuration permanente

### **Efficacité à long terme** :
- Solution permanente et automatique
- Plus fiable et précise
- Moins d'intervention manuelle nécessaire

---

## 🏆 **LA SOLUTION LA PLUS EFFICACE GLOBALEMENT** :

### **Configuration NTP (Network Time Protocol) + Fuseau horaire**

C'est la combinaison gagnante car elle résout le problème définitivement et automatiquement.

---

## 🚀 **PROCÉDURE COMPLÈTE ÉTAPE PAR ÉTAPE** :

### **Étape 1 : Vérifier l'état actuel**
```bash
# Voir l'heure actuelle et le fuseau horaire
timedatectl status
```

### **Étape 2 : Configurer le fuseau horaire (CRUCIAL)**
```bash
# Lister les fuseaux horaires disponibles
timedatectl list-timezones | grep -i europe

# Définir ton fuseau horaire (exemple pour Paris)
sudo timedatectl set-timezone Europe/Paris
```

### **Étape 3 : Activer la synchronisation automatique NTP**
```bash
# Activer NTP (Network Time Protocol)
sudo timedatectl set-ntp true

# Vérifier que c'est bien activé
timedatectl status
```

### **Étape 4 : Forcer une synchronisation immédiate**
```bash
# Synchroniser manuellement une première fois
sudo systemctl restart systemd-timesyncd

# Vérifier la synchronisation
timedatectl timesync-status
```

---

## 🛠 **COMMANDE ULTIME TOUT-EN-UN** :

Si tu veux tout configurer d'un coup (méthode la plus rapide) :

```bash
# Remplace "Europe/Paris" par ton fuseau horaire
sudo timedatectl set-timezone Europe/Paris && \
sudo timedatectl set-ntp true && \
sudo systemctl restart systemd-timesyncd && \
timedatectl status
```

---

## ✅ **POURQUOI CETTE MÉTHODE EST LA PLUS EFFICACE** :

| Aspect | Avantage |
|--------|----------|
| **Automatique** | Plus besoin d'y penser |
| **Précision** | Synchronisé avec serveurs atomiques |
| **Permanent** | Survit aux redémarrages |
| **Standard** | Méthode recommandée par systemd |
| **Simple** | Quelques commandes seulement |

---

## 🔧 **SOLUTIONS ALTERNATIVES (si NTP ne fonctionne pas)** :

### **Méthode manuelle rapide** :
```bash
# Définir l'heure manuellement (moins efficace)
sudo timedatectl set-time "2024-01-15 14:30:00"
```

### **Vérification finale** :
```bash
# Vérifier que tout fonctionne
timedatectl
```

---

## 💡 **CONSEIL PRO** :
Une fois NTP configuré, ton système restera synchronisé en permanence avec une précision de quelques millisecondes. C'est la méthode utilisée par tous les serveurs professionnels.


