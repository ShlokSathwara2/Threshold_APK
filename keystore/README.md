# Threshold signing keystore

This keystore is the permanent signing key for the Threshold Android app.
It replaces the original debug key, which was lost. Keep it in this repo so it
is never lost again.

- File: `keystore/threshold-release.keystore`
- Alias: `androiddebugkey`
- Store password: `android`
- Key password: `android`
- SHA-1: `C0:05:6F:8B:2B:A1:FB:40:B3:EB:60:6B:69:9B:25:46:67:88:8C:1D`

## How it is used

Gradle signs `assembleDebug` with `~/.android/debug.keystore`. On a fresh
machine, install this keystore there first:

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.android" | Out-Null
Copy-Item .\keystore\threshold-release.keystore "$env:USERPROFILE\.android\debug.keystore" -Force
```

Then build normally (`.\gradlew.bat assembleDebug`) and verify:

```powershell
apksigner verify --print-certs app-debug.apk   # expect SHA-1 C0:05:6F:8B:...
```

Every Threshold release must be signed with this file. Any other key breaks
in-place updates.
