# Dedicated Debian account for AI agents

Use `aiinspect` for diagnostics. Run the setup commands as root (`su -`).

```sh
apt install sudo
adduser --disabled-password --comment "AI diagnostics" aiinspect
usermod -aG systemd-journal,adm aiinspect
visudo -f /etc/sudoers.d/aiinspect-read
```

Add this rule in `visudo`:

```sudoers
aiinspect ALL=(root) NOPASSWD: NOSETENV: /usr/bin/cat, /usr/bin/ls, /usr/bin/stat, /usr/bin/readlink
```

Set the required permissions and verify:

```sh
chmod 0440 /etc/sudoers.d/aiinspect-read
visudo -c
```

Give the agent access through an SSH public key in `/home/aiinspect/.ssh/authorized_keys`. It can then inspect services, logs, and protected files:

```sh
systemctl status ssh.service --no-pager
journalctl -u ssh.service --no-pager
sudo -n cat -- /etc/ssh/sshd_config
sudo -n ls -la -- /root
```

**Security boundary:** This account can read secrets, including private keys and credentials, so treat it as highly trusted. Do not add it to the `sudo` group. Use a separate administrator account to approve and perform installations or configuration changes.