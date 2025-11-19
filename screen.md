# Screen
To manage processes in the background and recover them using `screen` on Linux, follow these steps:

### **1. Install Screen (if not already installed)**
```bash
sudo apt install screen  # Debian/Ubuntu
sudo yum install screen  # CentOS/RHEL
```

### **2. Start a New Screen Session**
```bash
screen -S session_name  # Replace 'session_name' with a descriptive name
```
- This opens a new terminal where you can run your process.

### **3. Run Your Process in the Screen Session**
Execute your command in the screen terminal (e.g., a long-running script or server):
```bash
./your_script.sh
```

### **4. Detach from the Screen Session**
Press `Ctrl + A` followed by `D` to detach. Your process continues running in the background.

---

### **Recover/Reattach to the Session**
#### List active sessions:
```bash
screen -ls
```
#### Reattach to a session:
```bash
screen -r session_name  # Use the name from step 2
```

---

### **Advanced Usage**
#### Start a detached session directly:
```bash
screen -dmS session_name your_command
```
Example:
```bash
screen -dmS my_server python3 app.py
```

#### Reattach to a shared session (multiple users):
```bash
screen -x session_name  # Allows simultaneous viewing/control
```

#### End a Screen Session:
- **Inside the session**: Run `exit` or press `Ctrl + D`.
- **From outside**:  
  ```bash
  screen -XS session_name quit  # Forcefully terminate
  ```

---

### **Key Shortcuts in Screen**
- `Ctrl + A + D`: Detach from session.  
- `Ctrl + A + C`: Create a new window (tab) within the session.  
- `Ctrl + A + N`: Switch to the next window.  
- `Ctrl + A + P`: Switch to the previous window.  
- `Ctrl + A + ?`: Show all shortcuts.

---

### **Why Use Screen?**
- Persists processes after SSH disconnects.  
- Allows reattachment from different terminals.  
- Maintains multiple terminal sessions in one window.

By using `screen`, you ensure your processes run uninterrupted and are easily recoverable.
