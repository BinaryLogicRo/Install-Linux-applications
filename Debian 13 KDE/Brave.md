# Brave Browser

## KDE Wallet issue

Somehow, the default KDE wallet password is setup to something unknown. This creates an issue, where Brave Browser cannot access the wallet to store or retrieve passwords and every time it tries (e.g. on startup), it prompts for the wallet password.

To fix this, you need to delete the default KDE wallet and recreate it with the password of the account that is currently logged in. This allows the KDE Wallet Manager to automatically unlock the wallet using the account password on login, eliminating the repeated prompts from Brave Browser.

### Steps to delete and recreate the KDE wallet:

1. Close KWalletManager.

```bash
kquitapp6 kwalletd6
```

2. Back up the existing wallet rather than deleting it

```bash
mkdir -p ~/kwallet-backup
mv ~/.local/share/kwalletd/kdewallet.kwl ~/kwallet-backup/
```

3. Start KWalletManager again.

```bash
kwalletmanager
```

and create:

```
Name:       kdewallet
Encryption: Classic / Blowfish
Password:   EXACTLY your current Linux login password
```

4. Then log out → SDDM → log back in.

After login, opening KWalletManager should show that the wallet is automatically unlocked without prompting for the password.

5. Delete the backup of the old wallet if everything is working correctly.

```bash
rm -rf ~/kwallet-backup
```