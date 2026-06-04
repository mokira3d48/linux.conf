## Resize windows

Let’s troubleshoot this step by step. The config‑based pixel size can fail for several reasons. Below I’ll cover the most common causes, then give you a foolproof alternative using character‑based geometry (which is what the “default terminal size” really is).

---

### 🔍 First, check why the config didn’t take effect

**1. Did you set `window_state` correctly?**  
Open `~/.config/terminator/config` and verify the `[global_config]` section looks *exactly* like this:
```ini
[global_config]
  window_state = normal
  size = 958, 564
```
- No spaces around the comma (`958, 564`, not `958 , 564`).
- No other `window_state` entries later that might override it (e.g., a `[layouts]` section).

**2. Are there leftover Terminator processes?**  
Sometimes the daemon hangs onto old settings.  
```bash
killall -9 terminator
```
Then launch Terminator again.

**3. Does the size get applied but immediately overridden?**  
Open Terminator, immediately run this *inside Terminator*:
```bash
xdotool getactivewindow windowsize
```
If the numbers match your setting, the config works but something (like a layout) resizes the window after startup. If they don’t match, the `size` line is being ignored.

---

### ✅ The foolproof alternative: match the **character** size (recommended)

The Kali default terminal isn’t defined by exact pixels – it’s defined by a **number of columns and rows**. You can find that out and tell Terminator to use the same character grid.

**Step 1 – Find the default terminal’s character size**
- Open Kali’s default terminal (the one you want to copy).
- Run:
  ```bash
  tput cols && tput lines
  ```
  or
  ```bash
  echo $COLUMNS $LINES
  ```
- You’ll get something like `80 24` or `100 37`. Remember these two numbers.

**Step 2 – Launch Terminator with that exact grid**  
Just add the `--geometry` flag when you start Terminator:
```bash
terminator --geometry=80x24
```
(replace `80x24` with your numbers – **width x height in characters**, no space)

This will open Terminator with exactly the same visible rows/columns, **independent of font or screen DPI**.

**Step 3 – Make it permanent**  
You have two ways:

**a) Edit the application launcher**
- Open `/usr/share/applications/terminator.desktop` with `sudo nano`.
- Find the `Exec=` line and change it to:
  ```
  Exec=terminator --geometry=80x24
  ```
- Save and relaunch from your menu.

**b) Use a shell alias** (if you start it from the terminal)
Add to `~/.bashrc`:
```bash
alias terminator='terminator --geometry=80x24'
```

---

### 🧩 If you absolutely want pixel‑perfect size via config

In rare cases, Terminator ignores the `size` parameter in `[global_config]` if you use a **layout** (even the default layout). You can force it by defining a custom layout with an explicit `size` for the first terminal:

1. In `~/.config/terminator/config`, add:
```ini
[layouts]
  [[default]]
    [[[window0]]]
      type = Window
      parent = ""
      size = 958, 564
    [[[terminal1]]]
      type = Terminal
      parent = window0
```

2. Set Terminator to always use this layout: in `[global_config]` add:
```ini
  layout = default
```

Now it will respect the pixel dimensions.

---

Let me know what happens when you try the `--geometry` method – it’s the cleanest way to get the exact “default terminal” look.
