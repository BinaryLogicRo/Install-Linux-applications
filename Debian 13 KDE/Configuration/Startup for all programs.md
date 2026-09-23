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