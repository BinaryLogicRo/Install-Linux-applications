# Install KiCad 10

This setup allows a single user to install and use KiCad 10 on their system.

The final result will be a fully functional KiCad desktop application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu.

Note: the default (Debian repository) version of KiCad is not used in this setup; KiCad is installed manually from the official AppImage.

## 1. Download KiCad

Download the KiCad AppImage for Linux from: [https://www.kicad.org/download/linux/](https://www.kicad.org/download/linux/).

The downloaded file will be a tarball (e.g., `kicad-10.0.6-x86_64.AppImage.tar`).

Direct download link for version **10.0.6**:

```bash
cd ~/Downloads
wget https://kicad-downloads.s3.cern.ch/appimage/stable/kicad-10.0.6-x86_64.AppImage.tar
```

## 2. Extract the tarball

Using `Ark`, extract the downloaded tarball into the `/home/user/Applications/KiCAD 10/` directory.

This will create the `/home/user/Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage` file.

Make sure the AppImage is executable:

```bash
chmod +x ~/"Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage"
```

## 3. Run KiCad

Navigate to the `KiCAD 10` folder and double-click the `kicad-10.0.6-x86_64.AppImage` executable to start the application.

## 4. Create a desktop shortcut

1. Download the KiCad icon:

```bash
mkdir -p ~/.local/share/icons
wget -O ~/.local/share/icons/kicad.svg https://gitlab.com/kicad/code/kicad/-/raw/10.0/resources/linux/icons/hicolor/scalable/apps/kicad.svg
```

2. Create the desktop shortcut file:

```bash
mkdir -p ~/.local/share/applications
nano ~/.local/share/applications/kicad.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=KiCad
GenericName=EDA Suite
Comment=Suite of tools for schematic design and circuit board layout
Exec="/home/user/Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage" %f
Icon=/home/user/.local/share/icons/kicad.svg
Terminal=false
StartupWMClass=kicad
Type=Application
Categories=Science;Electronics;
Keywords=kicad;eda;pcb;schematic;electronics;
SingleMainWindow=true
```

3. Refresh the menu

```bash
kbuildsycoca6
```

**Note:** when updating KiCad to a newer version, replace the AppImage in `/home/user/Applications/KiCAD 10/` and update the file name in the `Exec=` line of the `kicad.desktop` file.
