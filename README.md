# Blade client launcher

Electron launcher for Blade client: Premium (Microsoft) and Cracked (offline) accounts, any Minecraft version from Mojang's list, mods, video settings, and in-app logs.

## Run it

1. Install Node.js 18 or newer.
2. In this folder run `npm install`, then `npm start`.

Blade downloads the right Java for each Minecraft version the first time you play it, so you don't need to install Java yourself. The first launch of a version also downloads the game files, so give it a few minutes.

## Sections

- Play: pick a version and press Play.
- Premium: sign in with Microsoft.
- Cracked: choose a username for offline mode.
- Mods: add mod files, turn mods on and off, search Modrinth, install the performance pack (Sodium, Lithium, C2ME, ModernFix, Krypton and more), and choose the Fabric loader (Minecraft 1.14 and newer).
- Friends and Chat: add friends, accept requests, see who is online, global chat and direct messages. Needs the Blade server (see below).
- Blade mod: the in-game mod (`mod/`, Minecraft 1.21.11, 1.21.4 and 1.21.1) adds the Blade title screen, Right Shift PvP HUD, red and black menus, wings, blade, friends and chat. See `mod/README.md` to build it, then press Install Blade mod on the Mods page.
- Cosmetics: aura, cape, trail, and four emote slots. Saved to `config/blade/profile.json` in the game folder for the in-game Blade mod to read.
- Game: window size, fullscreen, max FPS, render distance, VSync, particles, entity shadows, and Java tuning.
- Client: theme (Blade, Crimson fade, Abyss fade), memory, Java path, snapshots and old versions, minimize on launch, and the game folder.
- Logs: everything the launcher and the game print. Use Copy to share it when something breaks.

## Chat and friends server

The server is in `server/` and has no dependencies.

1. Run `node server/server.js` (set `PORT` to change the port; it defaults to 3000). Test it with `node server/test.js`.
2. For friends to find each other, run it on a machine everyone can reach and put its address (for example `http://your-host:3000`) in the Server box on the Friends page.

Names are claimed first come, first served. The launcher remembers the key for your name, so nobody else can take it afterwards. Use HTTPS (put the server behind a reverse proxy) before sharing it publicly.

## Structure

- `main.js` creates the window and wires up IPC.
- `preload.js` exposes a small, safe API to the UI.
- `src/game.js` signs in and launches the game.
- `src/java.js` downloads Mojang's Java runtimes.
- `src/versions.js` loads the version list and prepares Fabric.
- `src/mods.js` manages the mods folder and talks to Modrinth.
- `src/options.js` writes video settings into `options.txt`.
- `src/social.js` talks to the chat and friends server.
- `src/store.js` saves settings, `src/log.js` keeps the log, `src/paths.js` and `src/util.js` are helpers.
- `renderer/` holds the UI.

## Next steps

- Remember the Microsoft login (encrypt the token with Electron's `safeStorage`).
- Forge support for 1.8.9 mods.
- Mod: cape and emotes in game, theming the vanilla title and pause screens.

## Installer and website

- **Windows installer:** the `Build Blade` workflow in `.github/workflows/release.yml` builds the three mod jars, puts them in `bundled/`, and builds `Blade-Client-Setup.exe` on GitHub. You do not need to run `npm install` on your PC. To build it on your PC instead, run `npm install` then `npm run dist` (the installer is created in `dist/`).
- **Public release:** push a tag such as `v1.0.0` and the workflow attaches the installer to a GitHub Release.
- **Website:** `docs/index.html` is a single page (red and black fade, embers, the sword). Turn on GitHub Pages: Settings, Pages, Source "Deploy from a branch", Branch `main`, folder `/docs`. Then edit the `DOWNLOAD_URL` line near the bottom of `docs/index.html` to your releases page.

- **Web installer page:** `docs/install.html` is the public install page (logo, steps, download button). Set `REPO` near the bottom of that file to your `OWNER/REPO`. The button downloads the newest `Blade-Client-Setup.exe` from your GitHub Releases.
- **Logo on the installer:** the Windows installer shows the sword logo in its icon, side banner and header (`build/icon.ico`, `build/installerSidebar.bmp`, `build/installerHeader.bmp`).

## Build the installer in VS Code

1. Open this folder in VS Code (File, Open Folder). The project and its `.cache` folder need about 3 GB of free space, so keep it on a drive with room (for example `D:\blade`).
2. Put the mod jars in `bundled/` (download them from the GitHub Actions run, see `mod/README.md`).
3. Terminal, Run Task: **1. Install packages**, then **3. Build the installer**.
4. The installer is created at `dist/Blade-Client-Setup.exe`.

If the build stops with "Cannot create symbolic link", turn on Developer Mode (Windows Settings, For developers) or run VS Code as administrator, then run the task again.
