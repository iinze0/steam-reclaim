# steam-reclaim

Steam uninstalls the game and keeps the leftovers.
Proton prefixes, shader caches, and `steamapps/common` folders sit there until the disk is gone.

This scans every Steam library on the machine and lists what is safe to delete.
Nothing is removed unless you pass `--apply`.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/iinze0/steam-reclaim/main/steam-reclaim -o ~/.local/bin/steam-reclaim
chmod +x ~/.local/bin/steam-reclaim
```

Needs Python 3.9+. No extra packages.

## Use

```bash
steam-reclaim
```

Example:

```text
Libraries
  /home/you/.local/share/Steam
  /mnt/games/SteamLibrary

Installed apps: 84

   SIZE  TYPE         PATH
 12.4 GB  compatdata   .../steamapps/compatdata/1234560
  3.1 GB  shadercache  .../steamapps/shadercache/1234560
  800 MB  common       .../steamapps/common/Some Old Game

3 leftover path(s) · 16.3 GB reclaimable

Delete nothing yet. Re-run with --apply to reclaim.
```

Then:

```bash
steam-reclaim --apply
```

| Flag | Meaning |
|:-----|:--------|
| `--apply` | delete the leftovers after a yes/no prompt |
| `-y` | skip the prompt (still requires `--apply`) |
| `--steam PATH` | Steam root if auto-detect misses it |

`STEAM_PATH` works too.

## What it considers leftover

- `steamapps/common/*` with no matching installed `appmanifest`
- `steamapps/compatdata/<appid>` when that app is not installed
- `steamapps/shadercache/<appid>` when that app is not installed

It keeps Steamworks redistributables and anything that still has a manifest.

## Notes

- Close Steam first if you plan to `--apply`.
- Extra libraries from `libraryfolders.vdf` are included.
- Flatpak Steam under `~/.var/app/com.valvesoftware.Steam` is detected.
- This does not touch saves inside a prefix that still belongs to an installed game.
