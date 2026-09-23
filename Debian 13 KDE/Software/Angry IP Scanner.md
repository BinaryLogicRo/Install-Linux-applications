# Angry IP Scanner

Angry IP Scanner is a fast network scanner for Linux.

**Warning:** The desktop session must be run under Wayland, otherwise the app will crash when trying to open it's settings window.

## Download

Go to [Github Repository of Angry IP Scanner](https://github.com/angryip/ipscan/releases) and download the `.deb` package for version **3.10.0**.

## Installation

```bash
cd ~/Downloads
sudo dpkg -i ipscan_3.10.0_all.deb
sudo apt update
sudo apt --fix-broken install
sudo apt install libswt-gtk-4-java libswt-cairo-gtk-4-jni libnotify-bin
```

**Explanation:**
- the `sudo apt --fix-broken install` command will install any missing dependencies, but not all of them
- the `sudo apt install libswt-gtk-4-...``` command will install the remaining required dependencies

## Launch

```bash
ipscan
```