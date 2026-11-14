# Remove Claude file manager menu entries

After Claude Desktop app is installed, Dolphin can add **Send to Claude** to file and folder menus and **Open in Claude Code** to folder menus.

## Disable the entries (recommended)

To hide them in Dolphin, open **Configure → Configure Dolphin → Context Menu**, uncheck the Claude services, and restart Dolphin.

## Remove the entries

1. Open Dolphin and visit `/home/user/.local/share/kio/servicemenus/`, and press **Enter**.

Delete these files if present:
   - `com.anthropic.Claude.SendTo.desktop`
   - `com.anthropic.Claude.OpenInCode.desktop`
2. Visit `/home/user/.local/share/kservices5/ServiceMenus/` the same way. Delete the same two filenames if present.
3. Visit `/home/user/.local/share/nautilus/scripts/`. Delete the scripts named `Send to Claude` and `Open in Claude Code` if present. These are for Nautilus rather than Dolphin, but they are part of the same integration. If the names are localized, open the suspected scripts in a text editor and remove only the ones containing `X-Claude-Generated=true`; do not run them.
4. Close every Dolphin window and reopen Dolphin. Right-click a file and a folder to check that the Claude entries are gone.
