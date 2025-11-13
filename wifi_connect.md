You can connect Ubuntu Server to WiFi using several methods, with **Netplan** being the most common for permanent configuration. Here's a comparison of your main options:

| Method | Best For | Key Steps |
| :--- | :--- | :--- |
| **Netplan (Permanent Setup)** | Standard, permanent configuration on installed systems. | Edit a YAML config file, then run `sudo netplan apply`. |
| **`nmcli` (Quick Connect)** | Quick, one-time connection if NetworkManager is installed. | Use `nmcli d wifi connect [SSID] password [password]`. |
| **Raspberry Pi Imager (Pre-install)** | Setting up WiFi during OS installation on a Raspberry Pi. | Use the "Advanced options" in the Raspberry Pi Imager tool. |

### 🖥️ Using Netplan for a Permanent Setup

Netplan is the default network configuration tool on Ubuntu Server. Here is the detailed process:

1.  **Find Your Wireless Interface Name**: Open a terminal and run `ip a` or `ls /sys/class/net`. Your WiFi interface typically starts with a "w" (e.g., `wlan0`, `wlp3s0`).
2.  **Edit the Netplan Configuration File**: Edit the YAML file in `/etc/netplan/` (common filenames include `50-cloud-init.yaml` or `00-installer-config.yaml`).
    ```bash
    sudo nano /etc/netplan/50-cloud-init.yaml
    ```
3.  **Configure the File**: Add a `wifis` section, ensuring correct indentation (use spaces, not tabs).
    ```yaml
    network:
      version: 2
      wifis:
        wlan0:  # Replace with your interface
          dhcp4: true
          optional: true
          access-points:
            "Your-WiFi-Name":
              password: "Your-WiFi-Password"
    ```
4.  **Apply the New Configuration**:
    ```bash
    sudo netplan apply
    ```
    Use `sudo netplan --debug apply` for troubleshooting.

### ⚡ Quick Connection with `nmcli`

If your server has **NetworkManager** installed (more common on Desktop versions), you can connect quickly with:

1.  **List Available Networks**:
    ```bash
    nmcli d wifi list
    ```
2.  **Connect to a Network**:
    ```bash
    nmcli d wifi connect Your-SSID password Your-Password
    ```

### 🛠️ Additional Tips and Troubleshooting

*   **On a Raspberry Pi**: The easiest method is to configure WiFi during the SD card flashing process using the **Raspberry Pi Imager**. Click the gear icon for "Advanced options" to pre-set your WiFi credentials.
*   **Check if WiFi is Blocked**: Sometimes the WiFi interface might be soft-blocked. Use `rfkill list` to check. If it's blocked, unblock it with `sudo rfkill unblock wifi`.
*   **Install `wpasupplicant`**: Some fresh Ubuntu Server installations might lack necessary software. If Netplan fails, try installing it: `sudo apt install wpasupplicant` and reboot.
*   **Fix YAML Formatting Errors**: The error `Invalid YAML: inconsistent indentation` means your file's spacing is incorrect. Double-check that you've used spaces consistently for indentation.
