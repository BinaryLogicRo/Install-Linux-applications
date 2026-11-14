# Create a swap file

This guide adds a 4 GiB swap file on Debian 12 (Bookworm). It is based on the [Linux Notepad swap-file guide](https://linuxnotepad.com/create-swap-as-a-file-in-linux.html), with the swappiness setting kept in a dedicated `/etc/sysctl.d/` file and set to `0`.

## Check the current setup

```bash
htop
```

htop provides a real-time view of memory and swap usage.

## Create and enable the file

For a 4 GiB example, run:

```bash
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
```

Enable the swap temporarily (optional):

```bash
sudo swapon /swapfile
sudo swapon --show
```

## Enable it at boot

Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

Add this line once:

```text
# Swap space
/swapfile none swap defaults 0 0
```

## Set swappiness to zero

Create `/etc/sysctl.d/90-swappiness.conf`:

```bash
sudo nano /etc/sysctl.d/90-swappiness.conf
```

Give it this content:

```text
vm.swappiness = 0
```

Apply and check the value without rebooting:

```bash
sudo sysctl --load=/etc/sysctl.d/90-swappiness.conf
sudo sysctl vm.swappiness
```

Swappiness `0` favors keeping application memory in RAM for as long as possible.
This setting can raise the risk of out-of-memory kills on some workloads, so adjust it if that happens.

## Verify after a reboot

```bash
sudo swapon --show
sudo sysctl vm.swappiness
```

`/swapfile` should appear in the swap list, and swappiness should be `0`.

## References

- [Linux Notepad: Create swap space as a file in Linux](https://linuxnotepad.com/create-swap-as-a-file-in-linux.html)
- [Debian `swapon(8)`](https://manpages.debian.org/bookworm/mount/swapon.8.en.html)
- [Debian `sysctl.d(5)`](https://manpages.debian.org/bookworm/systemd/sysctl.d.5.en.html)
- [Debian `sysctl(8)`](https://manpages.debian.org/bookworm/procps/sysctl.8.en.html)
- [Linux kernel `vm.swappiness` documentation](https://docs.kernel.org/admin-guide/sysctl/vm.html#swappiness)
