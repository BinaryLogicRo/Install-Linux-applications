# Visual Studio Code

## Configure basic text encryption

If you accept this risk, you can set password-store to basic by opening the Command Palette (Ctrl+Shift+P) and running the Preferences: Configure Runtime Arguments command. This will open the argv.json file where you can add the setting "password-store":"basic".

Example:

```json
// This configuration file allows you to pass permanent command line arguments to VS Code.
// Only a subset of arguments is currently supported to reduce the likelihood of breaking
// the installation.
//
// PLEASE DO NOT CHANGE WITHOUT UNDERSTANDING THE IMPACT
//
// NOTE: Changing this file requires a restart of VS Code.
{
	// Use software rendering instead of hardware accelerated rendering.
	// This can help in cases where you see rendering issues in VS Code.
	// "disable-hardware-acceleration": true,

	// Allows to disable crash reporting.
	// Should restart the app if the value is changed.
	"enable-crash-reporter": true,

	// Unique id used for correlating crash reports sent from this instance.
	// Do not edit this value.
	"crash-reporter-id": "0f33d7d9-d979-4509-b1fa-9903cc96dfd4",

	// Configure basic text encryption
	// https://code.visualstudio.com/docs/configure/settings-sync#_not-recommended-configure-basic-text-encryption
	"password-store":"basic"
}
```

See more at [Visual Studio Code Documentation](https://code.visualstudio.com/docs/configure/settings-sync#_not-recommended-configure-basic-text-encryption)