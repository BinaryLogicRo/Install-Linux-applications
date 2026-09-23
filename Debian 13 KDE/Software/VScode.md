# Visual Studio Code

## Configure password storage with GNOME Keyring

KDE 6 is not fully supported by VS Code's Settings Sync; the documented workaround is `gnome-libsecret`, backed by GNOME Keyring. This gives a single-password setup: your SDDM login password automatically unlocks the keyring, so VS Code never prompt for a wallet password. Works under KDE Plasma Wayland since GNOME Keyring is just a session service.

**1. Install**

```bash
sudo apt install gnome-keyring libpam-gnome-keyring seahorse libsecret-tools
```

**2. Verify SDDM PAM support**

Debian 13's default `/etc/pam.d/sddm` already includes:

```text
-auth     optional pam_gnome_keyring.so
-session  optional pam_gnome_keyring.so auto_start
```

Check with `grep gnome_keyring /etc/pam.d/sddm`. If both lines are present, don't edit the file.

**3. Create the default `Login` keyring (one-time)**

Run `seahorse`, then under **Passwords**:

1. Create a keyring named `Login` if it doesn't exist.
2. Set its password to exactly your Linux login password.
3. Make `Login` the default keyring.

From then on: SDDM password → `pam_gnome_keyring` → `Login` keyring unlocked automatically → apps use it with zero prompts.

**4. Log out and back in**

Do a full KDE logout/login via SDDM, then confirm the daemon is running:

```bash
pgrep -af gnome-keyring-daemon
```

**5. Configure VS Code**

Test it once:

```bash
code --password-store=gnome-libsecret
```

If it works, make it permanent in `argv.json` (Command Palette → *Preferences: Configure Runtime Arguments*):

```json
{
    "password-store": "gnome-libsecret"
}
```

## References

See more at [Visual Studio Code Documentation](https://code.visualstudio.com/docs/configure/settings-sync#_recommended-configure-the-keyring-to-use-with-vs-code)