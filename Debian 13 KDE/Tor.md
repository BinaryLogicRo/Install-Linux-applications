# Install Tor Browser

This setup allows a single user to install and use Tor Browser on their system.

The final result will be a fully functional Tor Browser application with a convenient desktop shortcut and icon integrated into the KDE Plasma application menu.

## 1. Download Tor Browser

Download Tor Browser for Linux from: [https://www.torproject.org/download/](https://www.torproject.org/download/).

The downloaded file will be a tarball (e.g., `tor-browser-linux-x86_64-13.5.6.tar.xz`).

## 2. Extract the tarball

Using `Ark`, extract the downloaded tarball into the `/home/user/Applications/` directory.

This will create the `/home/user/Applications/tor-browser/` folder containing the `Browser` directory and the `start-tor-browser.desktop` file.

## 3. Run Tor Browser

Navigate to the `/home/user/Applications/tor-browser/` folder and double-click `start-tor-browser.desktop` to start the application.

## 4. Create a desktop shortcut

The desktop shortcut is not required to be copied, symlinked or created manually, as the `start-tor-browser.desktop` file already adds the application to the KDE application menu.
