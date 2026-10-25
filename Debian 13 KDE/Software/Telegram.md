# Install Telegram

This setup allows a single user to install and use Telegram on their system.

The final result will be a fully functional Telegram desktop application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu.

## 1. Download Telegram

Download the Telegram for Linux from: [https://desktop.telegram.org/](https://desktop.telegram.org/).

The downloaded file will be a tarball (e.g., `td-setup-linux-x64-7.2.9.tar.xz`).

## 2. Extract the tarball

Using `Ark`, extract the downloaded tarball into the `/home/user/Applications/Telegram/` directory.

## 3. Run Telegram

Navigate to the extracted `Telegram` folder and double-click the `Telegram` executable to start the application.

## 4. Create a desktop shortcut

1. Copy the Telegram icon from this repository ([Assets/Telegram/telegram.png](Assets/Telegram/telegram.png)).

Run this from the root folder of this repository:

```bash
mkdir -p ~/.local/share/icons
cp "Debian 13 KDE/Software/Assets/Telegram/telegram.png" ~/.local/share/icons/telegram.png
```

2. Create the desktop shortcut file:

```bash
mkdir -p ~/.local/share/applications
nano ~/.local/share/applications/telegram-desktop.desktop
```

Paste this in:

```ini
[Desktop Entry]
Version=1.0
Name=Telegram
Comment=Official desktop version of Telegram messaging app
Exec=/home/user/Applications/Telegram/Telegram -- %u
Icon=/home/user/.local/share/icons/telegram.png
Terminal=false
StartupWMClass=TelegramDesktop
Type=Application
Categories=Chat;Network;InstantMessaging;Qt;
MimeType=x-scheme-handler/tg;x-scheme-handler/tonsite;
Keywords=tg;chat;im;messaging;messenger;sms;tdesktop;
SingleMainWindow=true
```

3. Refresh the menu

```bash
kbuildsycoca6
```