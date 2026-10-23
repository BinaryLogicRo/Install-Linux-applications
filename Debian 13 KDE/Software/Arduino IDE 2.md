# Install Arduino IDE 2

This setup allows a single user to install and use Arduino IDE 2 on their system, alongside the Arduino IDE 1.x installed from the Debian repository (`sudo apt install arduino`).

The final result will be a fully functional Arduino IDE 2 desktop application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu. Arduino IDE 1.x and Arduino IDE 2 will use separate boards, libraries and sketchbook folders, so they do not interfere with each other.

## 1. Download Arduino IDE 2

Download Arduino IDE 2 for Linux from: [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software).

Choose the "Linux ZIP file 64 bits (X86-64)" option. The downloaded file will be a zip archive (e.g., `arduino-ide_2.3.10_Linux_64bit.zip`).

## 2. Extract the archive

Using `Ark`, extract the downloaded archive into the `/home/user/Applications/` directory. This creates the `/home/user/Applications/arduino-ide_2.3.10_Linux_64bit/` folder.

Rename the extracted folder to `arduino-ide-2`, so the final path is `/home/user/Applications/arduino-ide-2/`.

## 3. Separate the Arduino IDE 2 data from Arduino IDE 1.x

By default, both Arduino IDE versions use the same folders:

- `~/.arduino15/` - installed boards (cores), tools, built-in libraries and package indexes
- `~/Arduino/` - sketchbook and libraries installed with the Library Manager

To keep the two versions isolated, configure Arduino IDE 2 to use its own folders. This must be done **before** starting Arduino IDE 2 for the first time, because on the first start it automatically installs the Arduino AVR boards and the built-in libraries.

Arduino IDE 1.x keeps its settings in `~/.arduino15/preferences.txt`, while Arduino IDE 2 keeps its settings in `~/.arduinoIDE/`, so only the data and sketchbook folders need to be changed.

1. Create the Arduino IDE 2 configuration file:

```bash
mkdir -p ~/.arduinoIDE
nano ~/.arduinoIDE/arduino-cli.yaml
```

Paste this in:

```yaml
directories:
  data: /home/user/.arduino-ide-2
  downloads: /home/user/.arduino-ide-2/staging
  user: /home/user/Arduino-IDE-2
```

2. Create the data and sketchbook folders:

```bash
mkdir -p ~/.arduino-ide-2 ~/Arduino-IDE-2
```

Arduino IDE 2 will use:

- `~/.arduino-ide-2/` - for boards (cores), tools, built-in libraries and package indexes
- `~/Arduino-IDE-2/` - as the sketchbook folder (sketches and libraries installed with the Library Manager)

Sketches from the Arduino IDE 1.x sketchbook (`~/Arduino/`) can still be opened in Arduino IDE 2 using `File` > `Open...`.

## 4. Run Arduino IDE 2

Navigate to the `/home/user/Applications/arduino-ide-2/` folder and double-click the `arduino-ide` executable to start the application.

On the first start, Arduino IDE 2 installs the Arduino AVR boards and the built-in libraries. Wait for the installation to finish before uploading sketches.

To confirm the separation, open `File` > `Preferences...` and check that the `Sketchbook location` is `/home/user/Arduino-IDE-2`.

## 5. Create a desktop shortcut

1. Copy the Arduino IDE 2 icon:

```bash
mkdir -p ~/.local/share/icons
cp ~/Applications/arduino-ide-2/resources/app/resources/icons/512x512.png ~/.local/share/icons/arduino-ide-2.png
```

2. Create the desktop shortcut file:

```bash
mkdir -p ~/.local/share/applications
nano ~/.local/share/applications/arduino-ide-2.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=Arduino 2.3
Comment=Open-source electronics prototyping platform
Exec=/home/user/Applications/arduino-ide-2/arduino-ide %U
Icon=/home/user/.local/share/icons/arduino-ide-2.png
Terminal=false
StartupWMClass=arduino-ide
Type=Application
Categories=Development;IDE;Electronics;
Keywords=arduino;ide;electronics;microcontroller;sketch;
SingleMainWindow=true
```

The Arduino IDE 1.x shortcut (`/usr/share/applications/arduino.desktop`) is not modified, so both shortcuts will be available in the application menu.

3. Refresh the menu

```bash
kbuildsycoca6
```
