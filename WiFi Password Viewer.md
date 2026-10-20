# Windows Wi-Fi Profiles & Saved Passwords

A simple guide to viewing saved Wi-Fi profiles and checking the saved
password on **your own Windows computer** using Command Prompt (CMD).

> **Use responsibly:** These commands can reveal saved Wi-Fi passwords.
> Use them only on your own computer or a device you are authorized to
> manage.

## 1. Open Command Prompt

1.  Press **Win + R**.
2.  Type `cmd`.
3.  Press **Enter**.

## 2. List saved Wi-Fi profiles

``` cmd
netsh wlan show profiles
```

This displays the Wi-Fi network profiles saved on the computer.

## 3. View one profile's details and saved password

Replace `YOUR_WIFI_NAME` with the exact profile name shown in the
previous command:

``` cmd
netsh wlan show profile name="YOUR_WIFI_NAME" key=clear
```

Look for a line similar to:

``` text
Key Content            : your_saved_password
```

If **Key Content** is not shown, Windows may not have a retrievable
saved password for that profile, or the profile/security setup may not
expose one.

### Example

If the profile name is `EBAUB_WiFi`, run:

``` cmd
netsh wlan show profile name="EBAUB_WiFi" key=clear
```

Keep the quotation marks if the Wi-Fi profile name contains spaces.

## 4. Command reference

  -----------------------------------------------------------------------------------------------
  Command                                                     Purpose
  ----------------------------------------------------------- -----------------------------------
  `netsh wlan show profiles`                                  Lists saved Wi-Fi profiles

  `netsh wlan show profile name="YOUR_WIFI_NAME"`             Shows details for one profile

  `netsh wlan show profile name="YOUR_WIFI_NAME" key=clear`   Shows profile details and, when
                                                              available, the saved key in
                                                              readable form
  -----------------------------------------------------------------------------------------------

## 5. About the `for /f` command

This one-line loop is sensitive to Windows' output format and language,
so it may fail to extract profile names correctly:

``` cmd
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profile') do @echo %j | findstr -i -v echo | netsh wlan show profile %j key=clear | findstr /c:"Profile" /c:"Key Content"
```

For beginners, the recommended method is to run
`netsh wlan show profiles`, then check one profile using Section 3. It
is easier to understand and troubleshoot.

If you put a `for /f` loop in a **`.bat` file**, use `%%i` and `%%j`
instead of `%i` and `%j`. In an interactive CMD window, use single
percent signs.

## 6. Troubleshooting

-   **"There is no wireless interface on the system"**: Check that the
    Wi-Fi adapter is enabled and available.
-   **Profile not found**: Copy the profile name exactly from
    `netsh wlan show profiles`.
-   **No "Key Content" line**: The saved key may not be available for
    that profile.
-   **Command output differs**: Windows display language and version can
    change the wording or layout.

## Quick reference

``` cmd
netsh wlan show profiles
netsh wlan show profile name="YOUR_WIFI_NAME" key=clear
```

**Tip:** Treat any displayed Wi-Fi password as private. Do not publish
it in screenshots, logs, or repositories.
