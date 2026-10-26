# Cannae Mod

A Windows desktop and command-line APK patcher for one supported build of a specific Unity Android game. Select the original game files and the address of the companion server, and it produces one signed APK. The server is a separate project; this application does not run it.

## Before you start

- Windows 10 or 11 (64-bit).
- The game installed on BlueStacks or on an Android phone or tablet, and opened once so that it has finished downloading its game files.
- **BlueStacks**: use BlueStacks 5.21.660.2003 with a Pie 64-bit instance, the tested setup. Do not update BlueStacks to 5.22 or later: the game stops running properly. Other versions, instance types and emulators are untested.
- **Phone or tablet**: a USB cable, or Android 11 or later to connect over Wi-Fi.
- The companion server app. Set it up after the backup, as its own guide describes.

Extract the whole release ZIP to a folder you can write to, such as the desktop, and run `CannaeMod.exe`. Keep the files in that folder together. Do not put the folder under `Program Files`, or the app cannot update itself. Python, Java and the Android SDK are not needed.

The app has four steps: **Connect**, **Back up**, **Patch** and **Install**. Any step can be skipped; for example, with APK files you already have, start at **Patch**.

## 1. Connect

Choose the device that has the game. The list shows the BlueStacks instances on this PC and the phones and tablets connected over USB or Wi-Fi. A device that is not ready says what to do; choose **Refresh** after you change something on it.

- **BlueStacks**: in the instance, open **Settings › Advanced** and turn on **Android Debug Bridge (ADB)**.

Phones and tablets need Google's Android platform tools first. Download them under **Device tools**; the download starts only after you accept the [Android SDK License Agreement](https://developer.android.com/studio/terms). Then turn on **Developer options** on the device (if you do not see them, tap **Build number** seven times in **Settings › About phone**) and connect it:

- **Over USB**: turn on **Developer options › USB debugging**, connect the cable and tap **Allow** on the device.
- **Over Wi-Fi**: follow the steps under **Mobile device over Wi-Fi**.

## 2. Back up

The patched game can only be installed after the original is uninstalled, and uninstalling deletes the downloaded game files. Once the original is gone, they cannot be downloaded again, so back them up first.

Close the game, then choose **Start backup**. The backup only reads from the device, and it is kept only if every file is verified.

The patched game downloads these files from the companion server, so the backup is for the server; the app cannot put the files back on a device. When the backup finishes, give the server app the path shown under **Backup folder to set in the server** (**Copy** copies it). The path stays the same when you back up again.

## 3. Patch

1. Under **Game files**, choose the original game's APK, XAPK or APKS file, or a folder holding them. After a backup, this is already filled in.
2. Under **Server address**, choose the server's address (see [Server address](#server-address)).
3. Under **Save to**, choose a folder, then choose **Build APK**.

### Server address

The server app lists its addresses under **Addresses for Cannae Mod**. Choose the same kind of address here and enter the same port.

| Choose | When the game runs |
|---|---|
| **BlueStacks** | in BlueStacks on the PC that runs the server |
| **Same Wi-Fi** | on a phone or tablet on the same home network as that PC |
| **DuckDNS** | anywhere, including away from home |
| **Custom** | somewhere that reaches the server by another name or IP address, such as a server on another PC |

Windows Firewall, the DuckDNS name and port forwarding on the router are set up in the server app; its guide explains each one.

**Check connection** shows whether the server answers at the address and, if not, what to check. For a DuckDNS name, it also checks that the name points to this PC. To find this PC's public address, it contacts `api.ipify.org`; nothing is contacted until you press the button.

### HTTPS with a DuckDNS name

A DuckDNS address carries the game's traffic, including its sign-in token, across the internet. Use HTTPS so that it is encrypted:

1. In the server app, turn on HTTPS for the DuckDNS name, as its guide describes.
2. In **Patch**, choose **DuckDNS**, enter the name and switch `http` to `https` in front of it. Keep the same port.

An APK already built for the name with `http` keeps working; to switch it to HTTPS, change its address as described next.

If the app or the game reports that the certificate is not valid, restart the server: it gets and renews its certificate only when it starts.

### Change the address later

The address in the APK is only the starting address; you do not need to patch again when it changes.

- **On the device**: when the game cannot reach the server, it asks for another address.
- **In the app**:
  1. In **Patch › Game files**, choose the APK this app built, or choose **Copy from device** to take it from the device chosen in **Connect**.
  2. Enter the new address and choose **Change address**.
  3. Install the new APK in **Install**. The game data is kept, and an address saved in the game is replaced by the new one.

## 4. Install

**Install** shows what is installed on the device chosen in **Connect** and offers the matching action: **Update the game** over the patched game, which keeps the game data; **Replace the original game…** over the original game, which needs the backup from step 2; or **Install** when the game is not installed. Without a connected device, the step shows how to install the APK yourself.

Then start the server, open the game and check that it signs in.

## Settings and command line

**Settings** (the gear button) shows the app version and checks for updates. The app contacts GitHub only when you choose **Check for updates**.

`CannaeMod-CLI.exe` does the same tasks without the window and reads the same settings file, `apk_mod_config.toml` in the app folder unless you choose another under **Settings › Settings file**. Run `CannaeMod-CLI.exe --help` for its commands.

## License

Cannae Mod is released under the MIT License (`LICENSE`). The components bundled with it keep their own licenses; `THIRD_PARTY.md` lists them and `_internal/legal` holds their license texts.
