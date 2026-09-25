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

## 4. Create the desktop shortcuts

The AppImage contains all KiCad tools. The first argument selects the tool to start (e.g. `eeschema`, `pcbnew`, `gerbview`); without it, the KiCad project manager is started.

A shortcut is created for the project manager and for each tool that opens KiCad files, so the file types can be associated with them in the next step.

1. Download the KiCad icons:

```bash
BASE=https://gitlab.com/kicad/code/kicad/-/raw/10.0/resources/linux/icons/hicolor/scalable
mkdir -p ~/.local/share/icons/hicolor/scalable/apps
for i in kicad eeschema pcbnew gerbview; do
  wget -O ~/.local/share/icons/hicolor/scalable/apps/$i.svg $BASE/apps/$i.svg
done
```

2. Create the desktop shortcut files:

```bash
mkdir -p ~/.local/share/applications
```

- KiCad (project manager):

```bash
nano ~/.local/share/applications/kicad.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=KiCad
GenericName=EDA Suite
Comment=Suite of tools for schematic design and circuit board layout
Exec="/home/user/Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage" kicad %f
Icon=/home/user/.local/share/icons/hicolor/scalable/apps/kicad.svg
Terminal=false
StartupWMClass=kicad
Type=Application
Categories=Science;Electronics;
MimeType=application/x-kicad-project;
Keywords=kicad;eda;pcb;schematic;electronics;
SingleMainWindow=true
```

- KiCad Schematic Editor:

```bash
nano ~/.local/share/applications/kicad-eeschema.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=KiCad Schematic Editor
GenericName=Schematic Capture Tool
Comment=Standalone schematic editor for KiCad schematics
Exec="/home/user/Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage" eeschema %f
Icon=/home/user/.local/share/icons/hicolor/scalable/apps/eeschema.svg
Terminal=false
StartupWMClass=eeschema
Type=Application
Categories=Science;Electronics;
MimeType=application/x-kicad-schematic;
Keywords=kicad;eeschema;schematic;electronics;
```

- KiCad PCB Editor:

```bash
nano ~/.local/share/applications/kicad-pcbnew.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=KiCad PCB Editor
GenericName=PCB Layout Editor
Comment=Standalone circuit board editor for KiCad boards
Exec="/home/user/Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage" pcbnew %f
Icon=/home/user/.local/share/icons/hicolor/scalable/apps/pcbnew.svg
Terminal=false
StartupWMClass=pcbnew
Type=Application
Categories=Science;Electronics;
MimeType=application/x-kicad-pcb;
Keywords=kicad;pcbnew;pcb;board;electronics;
```

- KiCad Gerber Viewer:

```bash
nano ~/.local/share/applications/kicad-gerbview.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=KiCad Gerber Viewer
GenericName=Gerber File Viewer
Comment=View Gerber and drill files
Exec="/home/user/Applications/KiCAD 10/kicad-10.0.6-x86_64.AppImage" gerbview %F
Icon=/home/user/.local/share/icons/hicolor/scalable/apps/gerbview.svg
Terminal=false
StartupWMClass=gerbview
Type=Application
Categories=Science;Electronics;
MimeType=application/x-gerber;application/vnd.gerber;application/x-excellon;application/x-gerber-job;
Keywords=kicad;gerbview;gerber;drill;excellon;electronics;
```

**Note:** the `Exec=` path contains a space (`KiCAD 10`), so it must be enclosed in double quotes, otherwise the shortcut will not start.

3. Refresh the menu

```bash
kbuildsycoca6
```

## 5. Register the KiCad file types

The AppImage does not register the KiCad file types (`.kicad_pro`, `.kicad_sch`, `.kicad_pcb`, etc.) with the system. They must be added manually, using the MIME type definitions from the KiCad source code.

1. Download the MIME type definitions:

```bash
BASE=https://gitlab.com/kicad/code/kicad/-/raw/10.0/resources/linux/mime
mkdir -p ~/.local/share/mime/packages
for m in kicad-kicad kicad-gerbers; do
  wget -O - $BASE/$m.xml.in | sed 's/@KICAD_MIME_ICON_PREFIX@//' > ~/.local/share/mime/packages/$m.xml
done
```

**Explanation:**
- the `sed` command removes the `@KICAD_MIME_ICON_PREFIX@` build placeholder from the source files

2. Download the file type icons:

```bash
BASE=https://gitlab.com/kicad/code/kicad/-/raw/10.0/resources/linux/icons/hicolor/scalable
mkdir -p ~/.local/share/icons/hicolor/scalable/mimetypes
for t in project schematic pcb footprint symbol worksheet; do
  wget -O ~/.local/share/icons/hicolor/scalable/mimetypes/application-x-kicad-$t.svg $BASE/mimetypes/application-x-kicad-$t.svg
done
```

3. Update the MIME database:

```bash
update-mime-database ~/.local/share/mime
```

4. Set the KiCad apps as default applications for the KiCad file types:

```bash
xdg-mime default kicad.desktop application/x-kicad-project
xdg-mime default kicad-eeschema.desktop application/x-kicad-schematic
xdg-mime default kicad-pcbnew.desktop application/x-kicad-pcb
xdg-mime default kicad-gerbview.desktop application/x-gerber application/vnd.gerber application/x-excellon application/x-gerber-job
```

5. Refresh the menu

```bash
kbuildsycoca6
```

6. Check the result

```bash
xdg-mime query default application/x-kicad-schematic
```

This should print `kicad-eeschema.desktop`. In Dolphin, `.kicad_pro`, `.kicad_sch` and `.kicad_pcb` files now show the KiCad icons and open with the matching KiCad app on double-click.

**Note:** when updating KiCad to a newer version, replace the AppImage in `/home/user/Applications/KiCAD 10/` and update the file name in the `Exec=` line of all the `kicad*.desktop` files.
