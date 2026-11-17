# Realtek Bluetooth drivers

This setup makes a Realtek USB Bluetooth adapter work on Debian 13 KDE.

The kernel already has the driver (`btusb` with `btrtl`), but it also needs firmware files that Debian ships in the `firmware-realtek` package. Without them, Bluetooth cannot be enabled in KDE System Settings.

Tested with a Realtek RTL8761BU adapter (generic USB Bluetooth dongle), USB ID `0bda:8771`.

## 1. Identify the adapter

```bash
lsusb | grep -i bluetooth
```

The output should contain `0bda:` (the Realtek vendor ID). For the tested adapter it contains `0bda:8771`.

## 2. Check that the firmware is missing

```bash
sudo journalctl -b -k -o cat --no-pager | grep -E 'RTL|firmware'
```

The kernel lists the firmware files it needs, for example `rtl_bt/rtl8761bu_fw.bin` and `rtl_bt/rtl8761bu_config.bin`. If they are missing, the output contains `not found` or `failed with error -2` next to these file names.

## 3. Check that the non-free-firmware component is enabled

`firmware-realtek` is in the `non-free-firmware` component of the Debian repository:

```bash
grep -r non-free-firmware /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
```

The output should contain at least one Debian `trixie` line with `non-free-firmware`. The Debian 13 installer usually enables it.

If the output is empty, open the sources file:

```bash
sudo nano /etc/apt/sources.list.d/debian.sources
```

Add `non-free-firmware` at the end of each `Components:` line, for example:

```text
Components: main non-free-firmware
```

If your system uses `/etc/apt/sources.list` instead, add `non-free-firmware` at the end of each `deb` line in that file.

## 4. Install the firmware

```bash
sudo apt update
sudo apt install firmware-realtek
```

Check that the firmware for the tested adapter is installed:

```bash
dpkg -L firmware-realtek | grep rtl8761bu
```

The output should list `rtl8761bu_fw.bin` and `rtl8761bu_config.bin` under `rtl_bt/`.

## 5. Reload the adapter

Unplug the adapter and plug it back in. The kernel loads the firmware only when the adapter is initialized.

## 6. Check the result

```bash
sudo journalctl -b -k -o cat --no-pager | grep -E 'RTL'
```

You should see the firmware loaded, with no `not found` errors:

```text
Bluetooth: hci0: RTL: examining hci_ver=0a hci_rev=000b lmp_ver=0a lmp_subver=8761
Bluetooth: hci0: RTL: rom_version status=0 version=1
Bluetooth: hci0: RTL: btrtl_initialize: key id 0
Bluetooth: hci0: RTL: loading rtl_bt/rtl8761bu_fw.bin
Bluetooth: hci0: RTL: loading rtl_bt/rtl8761bu_config.bin
Bluetooth: hci0: RTL: cfg_sz 6, total sz 30210
Bluetooth: hci0: RTL: fw version 0xdfc6d922
```

Open **System Settings → Bluetooth** and enable Bluetooth. It can now be turned on and finds nearby devices.

**Notes:**
- If Bluetooth still cannot be enabled, check whether it is blocked with `rfkill list bluetooth`. If it shows `Soft blocked: yes`, run `rfkill unblock bluetooth`.

## References

- [Debian Wiki: Bluetooth](https://wiki.debian.org/BluetoothUser)
- [Debian Wiki: Firmware](https://wiki.debian.org/Firmware)
- [Debian package: firmware-realtek](https://packages.debian.org/trixie/firmware-realtek)
