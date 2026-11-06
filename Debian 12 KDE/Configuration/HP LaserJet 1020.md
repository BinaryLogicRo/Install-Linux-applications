# HP LaserJet 1020

The HP LaserJet 1020 needs the `foo2zjs` driver and firmware uploaded to the printer each time it is powered on. USB detection or a completed print job does not confirm that the firmware loaded.

## 1. Install CUPS and the driver

```bash
sudo apt update
sudo apt install cups printer-driver-foo2zjs wget
sudo systemctl enable --now cups
```

## 2. Download the printer firmware

```bash
sudo getweb 1020
ls -l /lib/firmware/hp/sihp1020.dl
```

On Debian 12, `getweb` downloads the HP firmware from the Quirinux mirror and installs it at `/lib/firmware/hp/sihp1020.dl`. The download source is defined in `/usr/sbin/getweb`.

## 3. Fix automatic firmware loading

On Debian 12, the firmware loader can race with automatic printer configuration. In that case, the journal shows `loading HP LaserJet 1020 firmware ...` followed by `udev-configure-printer: failed to claim interface`, with no `download successful` message. See [Debian bug #1001440](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1001440).

Run this once to make the loader finish the transfer before exiting. It saves the original script as `/lib/udev/hplj1000.bak`:

```bash
sudo sed -i.bak '/^load_cups() {/,/^}/ s/^    ) &$/    )/' /lib/udev/hplj1000
```

Package upgrades may replace this script. If automatic firmware loading stops working after an upgrade, check whether this change is still present.

## 4. Add the printer

With USB connected, turn the printer off and back on. In KDE, open **System Settings → Printers** and add the detected HP LaserJet 1020. Select **HP LaserJet 1020 Foomatic/foo2zjs-z1 (recommended)** as the driver.

You can also add it at [http://localhost:631/admin](http://localhost:631/admin). CUPS asks for a Debian administrator account and password, not a separate printer password.

## 5. Verify printing

Print a test page from the printer settings or a page from Okular. To check whether the automatic firmware upload succeeded, inspect the journal:

```bash
sudo journalctl -b --no-pager | grep -E 'hplj1020|download successful|failed to claim interface'
```

Look for `download successful` after the printer is powered on. If the printer still does not print, upload the firmware manually with its USB URI. For this printer, the tested URI is `usb://HP/LaserJet%201020?serial=FN2X6AH`:

```bash
sudo env DEVICE_URI='usb://HP/LaserJet%201020?serial=FN2X6AH' \
  /usr/lib/cups/backend/usb 1 1 1 1 '' /lib/firmware/hp/sihp1020.dl
```

Use the URI shown in the CUPS printer page if the printer has a different serial number. The firmware transfer is temporary; the automatic loader should handle it again after each power cycle.
