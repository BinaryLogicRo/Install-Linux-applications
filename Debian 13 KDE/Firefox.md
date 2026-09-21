# Install Firefox

This setup allows a single user to install and use Firefox on their system.

The final result will be a fully functional Firefox desktop application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu.

Note: the default (Debian repository) version of Firefox is not required for this setup; it can remain installed alongside the manually downloaded version without conflict.

## 1. Download Firefox

Download Firefox for Linux from: [https://www.mozilla.org/firefox/new/](https://www.mozilla.org/firefox/new/).

The downloaded file will be a tarball (e.g., `firefox-156.0.tar.xz`).

## 2. Extract the tarball

Using `Ark`, extract the downloaded tarball into the `/home/user/Applications/Firefox/` directory.

## 3. Run Firefox

Navigate to the extracted `Firefox` folder and double-click the `Firefox` executable to start the application.

## 4. Create a desktop shortcut

1. Create the desktop shortcut file:

```bash
mkdir -p ~/.local/share/applications
nano ~/.local/share/applications/firefox.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=Firefox
Comment=Browse the World Wide Web
Exec=/home/user/Applications/Firefox/firefox %u
Icon=/home/user/Applications/Firefox/browser/chrome/icons/default/default128.png
Terminal=false
StartupWMClass=firefox
Type=Application
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml+xml;x-scheme-handler/http;x-scheme-handler/https;
Keywords=web;browser;internet;
SingleMainWindow=true
```

2. Refresh the menu

```bash
kbuildsycoca6
```
