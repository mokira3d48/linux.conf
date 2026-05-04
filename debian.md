## Screenshot app install
To install it on Debian open up terminal and run these 2 commands:
Bash:

```bash
sudo apt update
sudo apt install xfce4-screenshooter
```

If your account is root account (most likely isn't) you can omit sudo part.

```
Usage:
  xfce4-screenshooter [OPTION…]

Help Options:
  -h, --help               Show help options
  --help-all               Show all help options
  --help-gtk               Show GTK+ Options

Application Options:
  -c, --clipboard          Copy the screenshot to the clipboard
  -d, --delay              Delay in seconds before taking the screenshot
  -f, --fullscreen         Take a screenshot of the entire screen
  -m, --mouse              Display the mouse on the screenshot
  --no-border              Removes the window border from the screenshot
  -o, --open               Application to open the screenshot
  -r, --region             Select a region to be captured by clicking a point of the screen without releasing the mouse button, dragging your mouse to the other corner of the region, and releasing the mouse button.
  -s, --save               File path or directory where the screenshot will be saved, accepts png, jpg and bmp extensions. webp, jxl and avif are only supported if their respective pixbuf loaders are installed.
  -S, --show in folder     Show the saved file in the folder.
  -V, --version            Version information
  -w, --window             Take a screenshot of the active window
  --supported-formats      Lists supported image formats, results can vary depending on installed pixbuf loaders
  --display=DISPLAY        X display to use
```


## Installation and configuration of Screen Locking for XFCE desktop
Run the following command line:

```bash
sudo apt remove light-locker
sudo apt install xfce4-screensaver
```

And then, run the following command line:

```bash
xset s off -dpms
```

This command line disables both the X server's screen saver and the DPMS (Display Power Management Signaling) power-saving features. 

- `xset s off` turns off the screen saver (preventing the screen from blanking or activating a screensaver).
- `-dpms` disables DPMS, which normally allows the monitor to enter low-power states (standby, suspend, or off) after a period of inactivity.

The combined effect is to keep the display continuously active and powered on, without any automatic blanking or power-down. This is commonly used for presentations, watching movies, or any scenario where you want to prevent the screen from turning off due to inactivity.
