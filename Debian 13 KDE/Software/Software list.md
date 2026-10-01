# Software list

This is a list of recommended software for Debian 13 KDE. Command line tools are included in the [Default applications](#default-applications) section.

## Default applications

Install the every day essential tools:

```bash
sudo apt install tmux mc htop net-tools ncdu tree curl lsof
```

### System Information Tool

```bash
sudo apt install fastfetch --no-install-recommends
```

### Python Environment

```bash
sudo apt install python3 python3-pip python3-venv
```

## File manager

- [Krusader](https://krusader.org/) - advanced twin-panel file manager for KDE (Total Commander-like)

```bash
sudo apt install krusader
```

## Utilities

- [FileLight](https://apps.kde.org/filelight/) - visualize the disk usage on your computer

```bash
sudo apt install filelight
```

- [GParted](https://gparted.org/) - GNOME partition editor for creating, reorganizing, and deleting disk partitions

```bash
sudo apt install gparted
```

## Archiving tools

- [RAR/UNRAR](https://www.rarlab.com/download.htm)

Download the latest version of "RAR for Linux x64" package, extract it, and run `sudo make install` inside the extracted directory.

- [PeaZip](https://github.com/peazip/PeaZip/releases/) - open-source file archiver utility
