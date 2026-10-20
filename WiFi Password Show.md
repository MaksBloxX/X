# Windows WiFi Password Viewer

> **⚠️ DISCLAIMER:** This guide is intended **strictly for personal and educational use**. Retrieving WiFi passwords from computers you do not own or have explicit permission to access is **illegal** in most jurisdictions. The author assumes no liability for misuse.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Commands Reference](#commands-reference)
- [Step-by-Step Guide](#step-by-step-guide)
- [View All Saved Passwords at Once](#view-all-saved-passwords-at-once)
- [Output Explanation](#output-explanation)
- [Troubleshooting](#troubleshooting)
- [Security Tips](#security-tips)
- [Legal Notice](#legal-notice)

---

## Overview

Windows stores the credentials of every WiFi network you have ever connected to. These saved profiles include the **network name (SSID)**, **security type**, and the **password (pre-shared key)**.

Using the built-in `netsh` (Network Shell) command-line tool, you can view all saved WiFi profiles and retrieve their passwords in **plain text** — no third-party software required.

---

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **OS** | Windows 10 / 11 (also works on Windows 8/8.1) |
| **Access** | Administrator privileges (recommended) |
| **Tool** | Command Prompt (`cmd`) or PowerShell |
| **WiFi Adapter** | Must be present (desktop or laptop) |

---

## Commands Reference

| Command | Description |
|---------|-------------|
| `netsh wlan show profile` | Lists all saved WiFi network names |
| `netsh wlan show profile name="WiFiName"` | Shows detailed info for a specific network |
| `netsh wlan show profile name="WiFiName" key=clear` | Shows the **password** in plain text |
| `netsh wlan show interfaces` | Shows currently connected WiFi details |
| `netsh wlan show networks` | Scans and lists all nearby WiFi networks |
| `netsh wlan show drivers` | Shows WiFi adapter driver information |

---

## Step-by-Step Guide

### Step 1 — Open Command Prompt as Administrator

1. Press **`Win + R`** on your keyboard.
2. Type `cmd` and press **Ctrl + Shift + Enter** (for Admin mode).
3. Click **Yes** on the UAC prompt.

> 💡 You can also search for **"Command Prompt"** in the Start Menu → Right-click → **Run as administrator**.

---

### Step 2 — List All Saved WiFi Profiles

```cmd
netsh wlan show profile
```

**Example Output:**

```
Profiles on interface Wi-Fi:

Group policy profiles (read only)
---------------------------------
    <None>

User profiles
-------------
    All User Profile     : Najir
    All User Profile     : Home_WiFi_5G
    All User Profile     : Office_Net
```

---

### Step 3 — View Password for a Specific Network

```cmd
netsh wlan show profile name="Najir" key=clear
```

> ⚠️ Replace `Najir` with your actual WiFi name.
> If the name contains **spaces**, you **must** use double quotes: `"Home WiFi 5G"`.

---

### Step 4 — Find the Password in the Output

Scroll down to the **Security settings** section and look for **`Key Content`**:

```
Security settings
-----------------
    Authentication         : WPA2-Personal
    Cipher                 : CCMP
    Security key           : Present
    Key Content            : MySecretPassword123
```

> ✅ **`Key Content`** = Your WiFi Password

---

## View All Saved Passwords at Once

To dump **every** saved WiFi profile and its password in one go, paste this single line:

```cmd
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profile') do @echo %j | findstr -i -v echo | netsh wlan show profile %j key=clear | findstr /c:"Profile" /c:"Key Content"
```

**Example Output:**

```
    All User Profile     : Najir
    Key Content            : MySecretPassword123
    All User Profile     : Home_WiFi_5G
    Key Content            : Home@2024!Secure
    All User Profile     : Office_Net
    Key Content            : Off1ceP@ss
```

---

## Output Explanation

Here is what each important field means in the full profile output:

| Field | Meaning |
|-------|---------|
| **Profile name** | The WiFi network name (SSID) |
| **Connection mode** | `Connect automatically` or `Connect manually` |
| **Authentication** | Security type: `Open`, `WPA2-Personal`, `WPA3-Personal`, etc. |
| **Cipher** | Encryption method: `CCMP` (AES), `TKIP`, or `None` |
| **Security key** | `Present` = has a password, `Absent` = open network |
| **Key Content** | 🔑 **The actual WiFi password in plain text** |

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| **`netsh` is not recognized** | Make sure you are in CMD, not PowerShell ISE. Try running `C:\Windows\System32\netsh.exe` directly. |
| **No profiles listed** | Your PC has never connected to any WiFi network, or the WiFi adapter is disabled. |
| **`Key Content` is blank** | The network is **open** (no password) or the profile is managed by Group Policy. |
| **"The wireless interface is not available"** | Your WiFi adapter is turned off or not installed. Enable it from **Settings → Network & Internet → Wi-Fi**. |
| **Access Denied** | Run CMD as **Administrator** (right-click → Run as administrator). |
| **Profile name with special characters fails** | Always wrap the name in double quotes: `name="My WiFi (5G)"` |

---

## Security Tips

- 🔒 **Delete old profiles** you no longer need:
  ```cmd
  netsh wlan delete profile name="OldWiFiName"
  ```
- 🛡️ **Change your WiFi password** regularly to prevent unauthorized access.
- 🚫 **Never share** your saved passwords with untrusted individuals.
- 💻 **Lock your computer** when stepping away to prevent others from running these commands.

---

## Legal Notice

> - This guide is for **recovering your own forgotten WiFi passwords** on your own devices.
> - Accessing saved credentials on a computer **you do not own** without permission is a violation of computer fraud laws (e.g., CFAA in the US, IT Act in India/Bangladesh).
> - The author is **not responsible** for any illegal or unethical use of this information.
>
> **Use responsibly. Stay legal.**

---

## License

This guide is provided **as-is** for educational purposes. No warranty is expressed or implied.
