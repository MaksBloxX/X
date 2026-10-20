# Windows Wi-Fi Profiles & Saved Passwords

A simple guide to viewing saved Wi-Fi profiles and retrieving the saved
password on **your own Windows computer** using Command Prompt (CMD).

> **⚠️ Use responsibly:** These commands can reveal saved Wi-Fi passwords.
> Use them only on your own computer or a device you are authorized to
> manage. Do not share or publish retrieved passwords.

---



## 1. Open Command Prompt

1. Press **Win + R**.
2. Type `cmd`.
3. Press **Enter**.

> 💡 **Tip:** For best results, run as **Administrator**:
> Search **"cmd"** in the Start Menu → Right-click → **Run as administrator**.

---

## 2. List Saved Wi-Fi Profiles

```cmd
netsh wlan show profiles
```

This displays all Wi-Fi network profiles previously saved on the computer.

**Example output:**

```
User profiles
-------------
    All User Profile     : EBAUB_WiFi
    All User Profile     : Home_Network
    All User Profile     : Office 5G
```

---

## 3. View a Profile's Password

Replace `YOUR_WIFI_NAME` with the exact profile name from the previous step:

```cmd
netsh wlan show profile name="YOUR_WIFI_NAME" key=clear
```

Scroll down to the **Security settings** section and look for:

```
Key Content            : your_saved_password
```

> If **Key Content** is not shown, Windows may not have a retrievable
> saved password for that profile, or the security setup may not expose one.

### Example

If the profile name is `EBAUB_WiFi`:

```cmd
netsh wlan show profile name="EBAUB_WiFi" key=clear
```

> ⚠️ Keep the **quotation marks** if the profile name contains spaces:
> ```cmd
> netsh wlan show profile name="Office 5G" key=clear
> ```

---

## 4. Command Reference

| Command | Purpose |
|---------|---------|
| `netsh wlan show profiles` | Lists all saved Wi-Fi profiles |
| `netsh wlan show profile name="NAME"` | Shows details for one profile |
| `netsh wlan show profile name="NAME" key=clear` | Shows details **and** the saved password |
| `netsh wlan show interfaces` | Shows currently connected Wi-Fi info |
| `netsh wlan delete profile name="NAME"` | Deletes a saved profile |

---

## 5. View All Passwords at Once (Advanced)

This one-liner loops through all profiles and extracts passwords:

```cmd
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profile') do @echo %j | findstr -i -v echo | netsh wlan show profile %j key=clear | findstr /c:"Profile" /c:"Key Content"
```

> ⚠️ **Important caveats:**
> - This command is **sensitive to Windows display language and version**.
>   It may fail to extract profile names correctly on non-English systems.
> - For beginners, the recommended method is to use **Sections 2 and 3**
>   one profile at a time. It is easier to understand and troubleshoot.
> - If you put this loop in a **`.bat` file**, use `%%i` and `%%j`
>   instead of `%i` and `%j`. In an interactive CMD window, use single
>   percent signs.

---

## 6. Delete Old Profiles

Remove saved networks you no longer use to keep your credentials safe:

```cmd
netsh wlan delete profile name="Old_WiFi_Name"
```

To delete **all** saved profiles at once:

```cmd
netsh wlan delete profile name=*
```

> ⚠️ This will remove all saved Wi-Fi passwords. You will need to
> reconnect and re-enter passwords for any networks you use.

---

## 7. Troubleshooting

| Problem | Solution |
|---------|----------|
| **"There is no wireless interface on the system"** | Ensure your Wi-Fi adapter is enabled. Check **Settings → Network & Internet → Wi-Fi**. |
| **Profile not found** | Copy the profile name **exactly** from `netsh wlan show profiles`, including spaces and capitalization. |
| **No "Key Content" line** | The network may be open (no password), or the saved key may not be retrievable for that profile type. |
| **Command output looks different** | Windows display language and version can change the wording and layout of the output. |
| **Access Denied errors** | Run CMD as **Administrator** (right-click → Run as administrator). |

---

## Quick Reference

```cmd
netsh wlan show profiles
netsh wlan show profile name="YOUR_WIFI_NAME" key=clear
```

> 🔒 **Privacy reminder:** Treat any displayed Wi-Fi password as private.
> Do not publish it in screenshots, logs, or public repositories.
