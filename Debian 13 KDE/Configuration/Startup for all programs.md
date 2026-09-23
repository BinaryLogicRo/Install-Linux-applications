# Startup for all programs

## 1. Thunderbird

### 1.1 Place a `startup.sh` script in the Thunderbird application directory `/home/user/Applications/thunderbird/`.

```bash
nano /home/user/Applications/thunderbird/startup.sh
```

and paste:

```bash
#!/usr/bin/env bash
MOZ_ENABLE_WAYLAND=0 /home/user/Applications/thunderbird/thunderbird &
sleep 3
xdotool search --sync --onlyvisible --class thunderbird windowminimize %@
```

**Explanation:**
- the `MOZ_ENABLE_WAYLAND=0` ensures that the xdotool can interact with the Thunderbird window correctly when using Wayland.
- `sleep 3` ensures that there is enough time for the Thunderbird window to appear before xdotool tries to minimize it.

### 1.2 Make the `startup.sh` script executable.

```bash
chmod +x /home/user/Applications/thunderbird/startup.sh
```

### 1.3 Add the `startup.sh` script to your system's startup applications.

Create a `.desktop` file for Thunderbird in the `/home/user/.config/autostart/` directory.

```bash
nano /home/user/.config/autostart/start.thunderbird.desktop
```
and paste:
```ini
[Desktop Entry]
Name=Startup Script for Thunderbird
Comment=Start Thunderbird at login
Icon=/home/user/Applications/thunderbird/chrome/icons/default/default48.png
Exec=/home/user/Applications/thunderbird/startup.sh
Type=Application
Terminal=False
```

### 1.4 Start Thunderbird on the first monitor

A KWin rule in `~/.config/kwinrulesrc` is required to position the Thunderbird window on the left monitor:

| Property | Value |
|---|---|
| Window class | `Mail` / `thunderbird` |
| Window role | `3pane` (the main window only; compose and dialog windows use other roles) |
| Position | `0,60`, which is the top-left of the left monitor |
| Size | `1920×1036` |
| State | Maximized |

The window menu option was missing because Thunderbird draws its own title bar, so right-clicking it opens Thunderbird's menu instead of KDE's. Enter the rule through System Settings instead:

**Steps:**

1. Open **System Settings → Apps & Windows → Window Management → Window Rules**, then click **Add New…**
2. Fill in the matching section:
   - **Description:** `Thunderbird on left monitor`
   - **Window class (application):** `Exact Match` → `thunderbird`
   - **Match whole window class:** `No`
   - **Window types:** `Normal Window`
3. Click **Add Property…** and add these:

   | Property | Mode | Value |
   |---|---|---|
   | Window role | Exact Match | `3pane` |
   | Position | Apply Initially | `0`, `60` |
   | Size | Apply Initially | `1920`, `1036` |
   | Maximized horizontally | Apply Initially | Yes |
   | Maximized vertically | Apply Initially | Yes |

4. Click **Apply**.
5. Quit Thunderbird completely (**☰ → Quit**), then log out and back in.

The window role match keeps the rule to the main window, so new message windows will still open wherever you're working. **Apply Initially** only sets the starting position, so you can still move Thunderbird later. Your `startup.sh` and autostart entry don't need any changes.

If you'd like me to write the rule to `~/.config/kwinrulesrc` and reload KWin instead, say so and I'll do it.

**Check the rule in `~/.config/kwinrulesrc`:**

```bash
nano ~/.config/kwinrulesrc
```

you should have a new rule section for Thunderbird, similar to the following:

```ini
[General]
count=1
rules=e2319791-c7a2-4e4f-b342-bfe7f8948b03

[e2319791-c7a2-4e4f-b342-bfe7f8948b03]
Description=Thunderbird on left monitor
maximizehoriz=true
maximizehorizrule=3
maximizevert=true
maximizevertrule=3
position=0,60
positionrule=3
size=1920,1036
sizerule=3
types=1
windowrole=3pane
windowrolematch=1
wmclass=thunderbird
wmclassmatch=1
```

## 2 WhatsApp and Teams (Web Apps)

The `.desktop` files for WhatsApp and Teams web apps are created by Brave and can be found in `~/.local/share/applications/`.

If those `.desktop` files are recreated  or modified, you need to reapply all the steps for setting up the window rules and autostart entries for those web apps.

### 2.1 Add the `.desktop` files to startup applications

Find the `.desktop` files for WhatsApp and Teams web apps in `~/.local/share/applications/`.
Copy or move them to `~/.config/autostart/` to add them to startup applications.

### 2.2 Set up window rules for WhatsApp and Teams (Web Apps)

```bash
nano /home/user/.config/kwinrulesrc
```

and add new rules:

```ini
[General]
count=3
rules=e2319791-c7a2-4e4f-b342-bfe7f8948b03,f540a6b0-3237-44df-a91e-cfada7523efd,e55b1c3a-05de-46f4-8cd1-68346e708913

[e2319791-c7a2-4e4f-b342-bfe7f8948b03]
Description=Thunderbird on left monitor
maximizehoriz=true
maximizehorizrule=3
maximizevert=true
maximizevertrule=3
position=0,60
positionrule=3
size=1920,1036
sizerule=3
types=1
windowrole=3pane
windowrolematch=1
wmclass=thunderbird
wmclassmatch=1

[e55b1c3a-05de-46f4-8cd1-68346e708913]
Description=Teams on right monitor
position=3880,40
positionrule=3
types=1
wmclass=kadndpdhfiaigidpmcgmgabmbcjnjbgn
wmclassmatch=2

[f540a6b0-3237-44df-a91e-cfada7523efd]
Description=WhatsApp on right monitor
position=3840,0
positionrule=3
types=1
wmclass=hnpfjngllnobngcgfapefoaidbinmjnm
wmclassmatch=2
```

### 2.3 Create a shared startup script

```bash
mkdir -p /home/user/Applications/brave-apps
nano /home/user/Applications/brave-apps/startup.sh
```

Paste:

```bash
#!/usr/bin/env bash
APP_ID="$1"
/opt/brave.com/brave/brave-browser --profile-directory=Default --app-id="$APP_ID" &
sleep 5

JS=$(mktemp --suffix=.js)
cat > "$JS" <<EOF
for (const w of workspace.windowList()) {
    if (w.resourceClass.includes("$APP_ID")) w.minimized = true;
}
EOF

ID=$(qdbus6 org.kde.KWin /Scripting org.kde.kwin.Scripting.loadScript "$JS" "minimize-$APP_ID")
qdbus6 org.kde.KWin /Scripting/Script$ID org.kde.kwin.Script.run
sleep 1
qdbus6 org.kde.KWin /Scripting org.kde.kwin.Scripting.unloadScript "minimize-$APP_ID"
rm -f "$JS"
```

```bash
chmod +x /home/user/Applications/brave-apps/startup.sh
```

### 2.4 Point the autostart entries at the script

In both files, change only the Exec= line:

```ini
nano ~/.config/autostart/brave-hnpfjngllnobngcgfapefoaidbinmjnm-Default.desktop
```
```bash
Exec=/home/user/Applications/brave-apps/startup.sh hnpfjngllnobngcgfapefoaidbinmjnm
```
```ini
nano ~/.config/autostart/brave-kadndpdhfiaigidpmcgmgabmbcjnjbgn-Default.desktop
```
```bash
Exec=/home/user/Applications/brave-apps/startup.sh kadndpdhfiaigidpmcgmgabmbcjnjbgn
```

### 2.5 Test

Before logging out, you can test the script with WhatsApp already open:
```bash
/home/user/Applications/brave-apps/startup.sh hnpfjngllnobngcgfapefoaidbinmjnm
```