# Twitter Skill

[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md)

Use X/Twitter from Codex, Claude Code, and other AI agents through your own local account session.

Twitter Skill combines a Windows/macOS desktop app, a JSON CLI, and a standard Agent Skill. Agents can search X, work with multiple connected accounts, upload images, publish posts and replies, and delete posts without receiving your X cookies.

## Capabilities

| Capability | Details |
| --- | --- |
| Search | Search posts with X query operators, result count, product mode, and pagination cursor |
| Accounts | List connected accounts and use a default or explicitly selected account |
| Publishing | Publish text posts, replies, and posts with up to four uploaded images |
| Media | Upload JPEG, PNG, and WebP images |
| Management | Delete posts and verify local account status |
| Agent integration | Install one standard Skill for Codex, Claude Code, Cursor, and other agents supported by the `skills` CLI |

## Requirements

1. Install the latest Twitter Skill desktop app from [GitHub Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest).
2. Open the app, connect at least one X account, and keep the app running in the system tray.
3. Install [Node.js](https://nodejs.org/) so `npx` is available.

## Install the Agent Skill

Choose either method. Both install the same Skill globally.

### Run it yourself

Run this command in a terminal:

```bash
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y
```

### Ask your Agent to install it

Copy the following message into Codex, Claude Code, or another supported Agent:

```text
Install the Twitter Skill globally from https://github.com/chadyi/twitter-skill-releases by running:
npx skills add chadyi/twitter-skill-releases -s twitter-skill -g -y

After installation, verify that twitter-skill is installed, read its SKILL.md, and confirm whether the local Twitter Skill desktop app is ready. Do not request, print, or store X cookies.
```

Verify manually:

```bash
npx skills list -g
```

Update later:

```bash
npx skills update twitter-skill -g
```

## Use It

After installation, ask your Agent naturally:

```text
Search X for the latest posts about GPT Image 2 and return 20 results.
```

```text
Publish "hello" with this image using my default X account.
```

```text
Reply to this X post with a concise summary.
```

The Agent reads the Skill and invokes the local CLI. CLI responses are structured JSON so the Agent can reliably inspect results and errors.

## How It Works

```text
Codex / Claude Code / another Agent
  -> installed twitter-skill/SKILL.md
  -> local Twitter Skill CLI
  -> running desktop tray process
  -> short-lived server request plan
  -> X request from the user's local Electron session and network
```

- X login state stays in the desktop app's isolated local account partition.
- X requests are executed from the user's device rather than from a shared server IP.
- The Skill contains CLI usage instructions only. It does not contain cookies or private X request implementation details.
- The public repository contains the Agent Skill and release files only. Application and server source code remain private.

Twitter Skill uses the user's logged-in X web session and does not use the official X API.

## Desktop Downloads and Updates

Download the latest version from [Releases](https://github.com/chadyi/twitter-skill-releases/releases/latest).

| Platform | Package |
| --- | --- |
| Windows x64 | `Twitter-Skill-<version>-x64.exe` |
| macOS Universal | `Twitter-Skill-<version>-universal.dmg` |

Each release also contains update metadata, blockmaps, a macOS ZIP reserved for signed updates, and `SHA256SUMS.txt`. Windows downloads and installs updates automatically. The current unsigned macOS build only checks for a newer version; click **Go to download** in the app and install the latest DMG manually. macOS automatic installation will remain disabled until releases are signed and notarized with Apple Developer credentials.

## Troubleshooting

### macOS says the app is damaged and cannot be opened

First compare the downloaded file with `SHA256SUMS.txt` in the same Release and move `Twitter Skill.app` to `/Applications`. Then follow these steps in order:

1. Allow apps downloaded from outside the App Store.
   - macOS Ventura 13 or later: open **System Settings > Privacy & Security > Security**, then select **App Store and identified developers** under **Allow applications downloaded from**.
   - macOS Monterey 12 or earlier: open **System Preferences > Security & Privacy > General**, unlock the page, then select **App Store and identified developers**.
2. Open Terminal and remove the quarantine attribute from Twitter Skill only:

```bash
sudo xattr -dr com.apple.quarantine "/Applications/Twitter Skill.app"
```

3. Open `Twitter Skill.app` again. If macOS still blocks it, return to the same **Privacy & Security** or **Security & Privacy** page, click **Open Anyway**, and confirm **Open**.

Do not disable Gatekeeper globally. If the checksum does not match, delete that copy and download it again from the official GitHub Release.

### The Agent cannot find `twitter-skill`

Run `npx skills list -g`, reinstall the Skill, then restart the Agent so it reloads global Skills.

### The local CLI is unavailable

Open the Twitter Skill desktop app once and leave it running in the tray. The app creates the local CLI launcher during startup.

### The local X login has expired

Open the desktop app and reconnect the affected account. Never paste X cookies into the Agent or terminal.

### Publishing timed out

Check the account and post on X before retrying. A timed-out publishing request may already have completed.

## Repository Contents

```text
skills/twitter-skill/SKILL.md  Standard Agent Skill
GitHub Releases                Windows/macOS installers and update metadata
```

Application issues and release problems can be reported through [GitHub Issues](https://github.com/chadyi/twitter-skill-releases/issues).
