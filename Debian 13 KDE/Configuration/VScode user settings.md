# Configure Visual Studio Code user settings

This setup applies a set of user (global) settings to Visual Studio Code.

The settings are kept in the user `settings.json` file:

```text
~/.config/Code/User/settings.json
```

If the file already exists, the settings below overwrite the same settings in it, and all other settings in the file are kept. If the file does not exist, it is created with only these settings.

## 1. Close VS Code

Close all VS Code windows before changing the file.

## 2. Back up the current settings

```bash
cp ~/.config/Code/User/settings.json ~/.config/Code/User/settings.json.bak 2>/dev/null || true
```

## 3. Create a file with the new settings

```bash
nano /tmp/vscode-user-settings.json
```

Paste this in:

```json
{
    "git.autofetch": true,
    "git.confirmSync": false,
    "editor.wordWrap": "on",
    "editor.scrollbar.vertical": "visible",
    "editor.scrollbar.verticalScrollbarSize": 20,
    "editor.scrollbar.horizontalScrollbarSize": 20,
    "diffEditor.ignoreTrimWhitespace": false,
    "workbench.tree.indent": 20,
    "workbench.experimental.modernUI": false,
    "files.exclude": {
        "**/.git": false
    },
    "update.showReleaseNotes": false,
    "github.copilot.enable": {
        "*": true,
        "plaintext": true,
        "scminput": false,
        "markdown": true,
        "instructions": true
    },
    "github.copilot.nextEditSuggestions.enabled": true,
    "gitlens.currentLine.enabled": false,
    "gitlens.currentLine.pullRequests.enabled": false,
    "gitlens.codeLens.authors.enabled": false,
    "gitlens.codeLens.recentChange.enabled": false,
    "gitlens.hovers.currentLine.enabled": false,
    "gitlens.hovers.annotations.enabled": false,
    "gitlens.hovers.annotations.details": false,
    "gitlens.hovers.annotations.changes": false,
    "gitlens.fileAnnotations.preserveWhileEditing": false,
    "gitlens.views.remotes.files.layout": "tree",
    "gitlens.views.scm.grouped.views": {
        "branches": false,
        "remotes": false,
        "tags": false,
        "worktrees": false,
        "contributors": false,
        "repositories": false,
        "searchAndCompare": false,
        "launchpad": false,
        "commits": false,
        "stashes": false
    },
    "claudeCode.attachOpenFile": false,
    "liveServer.settings.donotShowInfoMsg": true
}
```

## 4. Merge the new settings into the user settings file

```bash
python3 - <<'EOF'
import json, os, re

new_file = "/tmp/vscode-user-settings.json"
settings_file = os.path.expanduser("~/.config/Code/User/settings.json")

# VS Code allows comments and trailing commas in settings.json, plain JSON does not
def strip_jsonc(text):
    out, i, in_str = [], 0, False
    while i < len(text):
        c = text[i]
        if in_str:
            out.append(c)
            if c == "\\":
                out.append(text[i + 1])
                i += 1
            elif c == '"':
                in_str = False
        elif c == '"':
            in_str = True
            out.append(c)
        elif text.startswith("//", i):
            i = text.find("\n", i)
            if i == -1:
                break
            continue
        elif text.startswith("/*", i):
            i = text.index("*/", i) + 2
            continue
        else:
            out.append(c)
        i += 1
    return re.sub(r",(\s*[}\]])", r"\1", "".join(out))

# Merge object values key by key, so existing entries that are not in the new settings are kept
def merge(target, source):
    for key, value in source.items():
        if isinstance(value, dict) and isinstance(target.get(key), dict):
            merge(target[key], value)
        else:
            target[key] = value

settings = {}
if os.path.exists(settings_file):
    with open(settings_file) as f:
        text = f.read()
    if text.strip():
        settings = json.loads(strip_jsonc(text))

with open(new_file) as f:
    merge(settings, json.load(f))

os.makedirs(os.path.dirname(settings_file), exist_ok=True)
with open(settings_file, "w") as f:
    json.dump(settings, f, indent=4, ensure_ascii=False)
    f.write("\n")

print("Updated", settings_file)
EOF
```

**Where:**
- each setting from `/tmp/vscode-user-settings.json` replaces the setting with the same name in `settings.json`
- settings with object values (for example `files.exclude`) are merged key by key: keys from the new settings overwrite the same keys, and other existing keys are kept
- all other settings already in `settings.json` are kept
- comments in the existing `settings.json` are removed; the original file is still available in `settings.json.bak`

## 5. Check the result

Start VS Code, open the Settings UI (`Ctrl+,`) and type `@modified` in the search box. It lists every setting that differs from its default value.

**Notes:**
- Settings for an extension (GitHub Copilot, GitLens, Claude Code, Live Server) take effect only when that extension is installed. Until then, VS Code shows them greyed out as unknown settings. This is harmless.
- When you change a setting in the Settings UI later, VS Code writes it back to this same file.

## References

See more at [Visual Studio Code Documentation](https://code.visualstudio.com/docs/configure/settings#_settings-json-file)
