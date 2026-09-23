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