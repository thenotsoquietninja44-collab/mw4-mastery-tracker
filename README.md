# MW4 Mastery Tracker

An offline Windows app for weapon camos, prestige, DMZ inventory, crafting, key guides, the Hajin map, and FOB upgrades.

## Download

Download **MW4-Mastery-Tracker-Setup-1.1.1.exe** from [Releases](https://github.com/thenotsoquietninja44-collab/mw4-mastery-tracker/releases/latest), run the installer, and start MW4 Mastery Tracker. Windows 10/11, 64-bit.

The installer contains the app and its images. Internet is optional: startup checks GitHub Releases for a newer version, but tracking and the map work offline. Available updates download in the background and ask before restarting. Your saved progress stays on your PC across updates.

The initial installer is unsigned. Windows may show an unrecognized publisher warning.

## Bring your existing progress

In your old HTML tracker, choose **Export backup**. Open the installed app and choose **Import backup** to bring it over. Other users start with blank progress. Export regularly for backups.

## Build from source

Install Node.js 24 LTS. In PowerShell in this repository:

```powershell
Expand-Archive -LiteralPath app-source.zip -DestinationPath . -Force
npm ci
node scripts/unpack-ui.cjs
Expand-Archive -LiteralPath .build/ui.zip -DestinationPath . -Force
Expand-Archive -LiteralPath ui-update.zip -DestinationPath . -Force
npm test
npm run build:win
```

The installer, update manifest (`latest.yml`), and blockmap are written to `release/`. Desktop source, tests and build scripts are in `app-source.zip`. UI source and local artwork are in the multipart `ui-assets` archive to keep each repository upload below 25 MB. Its SHA-256 is checked before extraction. After extraction, apply `ui-update.zip` for the current fixes; all UI HTML, CSS, and JavaScript is editable under `ui/`.

## Publish an update

Bump `version` in `package.json` and update the lockfile. Commit the new files, tag the commit `vX.Y.Z`, and push the tag. The Windows GitHub Actions workflow builds the installer and publishes its `.exe`, `.blockmap`, and `latest.yml` to a release. Publish all three together: installed apps check that release feed at startup.

Keep `appId`, `productName`, and the `app://tracker` origin stable so upgrades keep existing progress. No account, game login, or tracking service is used.

## Share loot maps

Open Map → Draw & mark to place colored pins or any item icon, add notes, and draw routes. Export loot map shares only your map notes. Import loot map merges notes without changing progress and skips duplicates. In Items & keys, Show map locations finds all matching item markers; markers link back to their item. Notes save automatically and travel with full progress backups.

## Automatic progress backups

Before installing an in-app update, the app saves and verifies a dated progress backup on this PC. If the backup fails, the update pauses. A snapshot is also saved when an update finishes downloading. Backups include inventory, weapon progress, crafting, and shared map notes.

Use Help → Restore from backup to select an automatic or exported JSON backup. Your current progress is backed up before restoring, and the app updates immediately. Help also includes Create progress backup and Open backups folder. Backups stay in the app’s progress-backups folder across updates and are never automatically deleted.
