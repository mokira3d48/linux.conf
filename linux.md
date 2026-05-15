## Disable touchscreen
If you have not already taken the advice from above, try what I did a few years ago?

G'day @S3l3n3 and welcome to linux.org :)

First of all check the output of

for the touchscreen entry

Then, in your file `/usr/share/X11/xorg.conf.d/40-libinput.conf`.


```sh
sudo nano /usr/share/X11/xorg.conf.d/40-libinput.conf
```

You may have a section that looks a bit like this

```conf
Section "InputClass"
        Identifier "libinput touchscreen catchall"
        MatchIsTouchscreen "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```

What you should do rather than just change on to off is alter it
to look like this

```conf
#Section "InputClass"
#        Identifier "libinput touchscreen catchall"
#        MatchIsTouchscreen "on"
#        MatchDevicePath "/dev/input/event*"
#        Driver "libinput"
#EndSection
```

Then save the file, reboot and see how you go.
Check the xinput output and you should see the previous reference removed.
If you try this and it works, I'll tell you a funny story about how I first used this.

## Changer la position de l'écran d'extension sous ubuntu 22.04 LTS
Pour configurer le déplacement des fenêtres vers un écran externe sous Kubuntu 22.04 sans utiliser la touche `Fn`, vous avez plusieurs options. Voici les méthodes les plus efficaces :

### 1. Utilisation des paramètres système KDE 🌐
- Allez dans **Paramètres système** > **Affichage et moniteur**.
- Dans l'onglet **Disposition**, vous pouvez définir la position relative des écrans (gauche, droite, au-dessus, en dessous).
- Assurez-vous que l'écran externe est activé et correctement positionné graphiquement.
- Appliquez les changements. Les fenêtres devraient maintenant se déplacer naturellement entre les écrans en fonction de leur position relative.

### 2. Raccourcis clavier personnalisés ⌨️
- Dans **Paramètres système** > **Raccourcis clavier** > **Gestions des fenêtres**.
- Cherchez des actions comme **Déplacer la fenêtre vers l'écran de droite** ou **Déplacer la fenêtre vers l'écran de gauche**.
- Attribuez-leur des raccourcis personnalisés (ex: `Meta+Shift+Flèche droite`).
- Utilisez ces raccourcis pour déplacer rapidement les fenêtres entre écrans.

### 3. Utilisation de lignes de commande avec xrandr 📟
- Installez `xrandr` si nécessaire (`sudo apt install x11-xserver-utils`).
- Utilisez des commandes pour ajuster la position des écrans, par exemple :
  ```bash
  xrandr --output HDMI-1 --right-of eDP-1
  ```
  (Remplacez `HDMI-1` et `eDP-1` par les noms de vos écrans, obtenus via `xrandr`).

### 4. Scripts de configuration d'écran dynamique
- Créez des scripts shell pour basculer entre différentes configurations d'écran.
- Par exemple, un script pour activer l'écran externe à droite :
  ```bash
  #!/bin/bash
  xrandr --output HDMI-1 --auto --right-of eDP-1
  ```
- Rendez le script exécutable (`chmod +x script.sh`) et assignez un raccourci clavier à son exécution dans les paramètres KDE.

### 5. Outils graphiques supplémentaires
- Des outils comme `arandr` (interface graphique pour xrandr) permettent de configurer visuellement la disposition des écrans et de sauvegarder des profils.
- Installez-le via :
  ```bash
  sudo apt install arandr
  ```

### 6. Gestion avancée avec KDE Plasma
- Plasma intègre des fonctionnalités étendues pour la gestion des écrans multiples.
- Explorez les options dans **Paramètres système** > **Affichage et moniteur** > **Comportement des fenêtres** pour affiner le déplacement.

### Notes importantes :
- Les noms des écrans (ex: `eDP-1`, `HDMI-1`) varient selon votre matériel. Utilisez `xrandr -q` pour les lister.
- Après toute modification via xrandr, les paramètres sont temporaires. Pour les rendre permanents, ajoutez les commandes à vos scripts de démarrage (ex: dans `~/.config/autostart/`).

Si vous rencontrez des problèmes avec le mode plein écran qui interfère, évitez le mode plein écran ou utilisez des gestionnaires de fenêtres alternatifs comme i3 ou Awesome. Pour des configurations très avancées, consultez la documentation de KDE ou des forums spécialisés.

---

## Méthode simple et temporaire pour blocker le clavier par défaut

Utilise directement `evdev` pour désactiver le clavier jusqu'au prochain redémarrage :

```bash
sudo evtest --grab /dev/input/event0
```

Laisse ce terminal ouvert — tant qu'il tourne, le clavier intégré est bloqué. **Ferme le terminal ou fais `Ctrl+C` pour le débloquer.**

---

Si tu veux une commande encore plus simple en arrière-plan :

```bash
# Bloquer
sudo evtest --grab /dev/input/event0 &> /dev/null &

# Débloquer
sudo kill $(pgrep evtest)
```

## Fix Of Chromium Starting Bug
Running Chromium on Linux often triggers this profile lock, which can be frustrating. It's usually not a serious problem, but a safety mechanism to protect your personal data from corruption. Let's walk through what's happening and get it fixed.

### 🤔 What's Actually Happening?

The error message indicates that Chromium has detected a **"Singleton" lock file** in your profile directory. This file acts as a "Do Not Disturb" sign, initially placed to prevent two copies of the browser from writing to your profile at the same time.

The error specifically mentions another computer named `kali`, which points to two common scenarios:

*   **Shared/Networked Home Directory**: If your Debian machine and the `kali` machine share the same network home directory (like NFS), a lock created on one will be visible and effective on the other.
*   **System Hostname Change**: If your current Debian machine was previously named `kali`, the old lock file still references that old hostname, making the system think it's a different computer.

In either case, a **stale lock file** left behind after an unexpected shutdown or crash is the most common culprit.

### 🔧 The Step-by-Step Fix

To resolve this, you can safely delete the stale lock files. The process is straightforward. **Important:** Please make sure no other Chromium processes are running before you begin.

1.  **Open a Terminal**: You can usually find this in your applications menu or launch it with a keyboard shortcut (often `Ctrl + Alt + T`).

2.  **Ensure All Chromium Processes are Closed**: It's a critical first step. Run this command to safely terminate any background processes that might be stuck.
    ```bash
    pkill -f chromium
    ```
    *The `-f` flag makes the command match the full process name, which is more thorough.*

3.  **Delete the Lock Files**: Chromium's lock files start with the word `Singleton` and are located in your profile directory. You can delete them with one command.
    ```bash
    rm -rf ~/.config/chromium/Singleton*
    ```
    *   This command (`rm`) removes (`-rf`) all files beginning with "Singleton" in the Chromium config directory.
    *   It will delete `SingletonLock`, `SingletonCookie`, and `SingletonSocket`, which are the three lock files commonly found.

4.  **Relaunch Chromium**: You should now be able to start Chromium normally from your applications menu or by typing `chromium` in the terminal.

### 💡 Additional Tips

*   **If you find the lock files keep reappearing**, especially after a system crash, it might be a sign to check your system's stability or how you're shutting down the browser.
*   **For systems with Snap packages**, the lock files might be in a different location. If the command above doesn't work, check `~/snap/chromium/common/chromium/` and delete the `Singleton*` files from there.

That should clear things up! Let me know if you run into any other issues.

---

> **Prérequis** : installe `evtest` si pas encore présent :
> ```bash
> sudo apt install evtest
> ```
