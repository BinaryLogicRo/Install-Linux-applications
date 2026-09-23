# Secure SSH folder and keys

This guide has been used as a reference for the migration of SSH configurations and keys from a Windows environment to a Debian 13 KDE environment.

## Secure the SSH directory

```bash
# Secure the SSH directory
chmod 700 ~/.ssh

# Ensure you own it and everything inside it
chown -R "$USER":"$(id -gn)" ~/.ssh
```

## Fix permissions

```bash
# Private keys, SSH config, and other sensitive files
find ~/.ssh -type f ! -name '*.pub' -exec chmod 600 {} \;

# Public keys may be readable
find ~/.ssh -type f -name '*.pub' -exec chmod 644 {} \;

# Optional: common non-secret files can be group/world-readable
chmod 644 ~/.ssh/known_hosts ~/.ssh/known_hosts.old 2>/dev/null || true
```

## Check the resulting permissions

```bash
ls -la ~/.ssh
```

You should generally see:

```
drwx------  3 user user  4096 Sep 14 12:23 .
drwx------ 24 user user  4096 Sep 22 10:01 ..
-rw-------  1 user user  7702 Aug 21 09:04 config
-rw-------  1 user user   419 Mar  5  2025 id_ed25519
-rw-------  1 user user   300 Mar  5  2025 id_ed25519.ppk
-rw-r--r--  1 user user   105 Mar  5  2025 id_ed25519.pub
-rw-r--r--  1 user user 17050 Sep 14 12:23 known_hosts
-rw-r--r--  1 user user 16122 Sep 10 10:33 known_hosts.old
```

**Where:**
- public keys are generally readable by anyone (permissions 644)
- private keys and sensitive files should be readable only by the owner (permissions 600)
- the SSH directory itself should be accessible only by the owner (permissions 700)