# KimaiClock

A native macOS menu bar app for Kimai 2 time tracking — pick a project, choose the activity, press play.

[![macOS](https://img.shields.io/badge/macOS-15%2B-1c1c1e?logo=apple&logoColor=white)](https://github.com/clemens-code/kimai-clock)
[![Swift](https://img.shields.io/badge/Swift-6.0-F05138?logo=swift&logoColor=white)](https://swift.org)
[![Kimai](https://img.shields.io/badge/Kimai-2.x-007acc)](https://www.kimai.org/)
[![License](https://img.shields.io/badge/License-MIT-gray)](./LICENSE.md)

![App Overview](https://github.com/Foraum-GmbH/kimai-clock/blob/main/assets/hero.jpeg?raw=true)

## Overview

KimaiClock brings your [Kimai](https://www.kimai.org/) time tracking into the macOS menu bar. Start, pause, annotate and switch timers without opening a browser window.

This is a fork of [Foraum-GmbH/kimai-clock](https://github.com/Foraum-GmbH/kimai-clock), adapted for teams that think in projects first:

- **Project-first search**: search for a project, then pick the activity from a drop-down. Labels read `project / activity`.
- **Tags**: select Kimai tags for a timesheet before you start it.
- **Drop-downs keep the popover open**: choosing an activity, tag or setting no longer closes the window.
- **Own releases**: installable via this repo's Homebrew tap or as a DMG from GitHub Releases.

> [!NOTE]
> KimaiClock requires Kimai 2. It does not support legacy Kimai 1 installations.

## Features

- **Menu Bar Control**: Start, pause, resume, and stop timesheets directly from the status item.
- **Project-First Selection**: Find a project by name and choose one of its activities, global activities included.
- **Tags and Descriptions**: Add tags and a note before starting or while paused; edit the note of a running timer.
- **Recent Activities**: Restart one of your last six project/activity pairs with a single click.
- **Server Sync**: Picks up a timer you started in the Kimai web UI or on your phone.
- **Smart Idle Detection**: Detects inactivity, asks what to do when you return, and adjusts the logged duration.
- **App Launch Reminders**: Prompts you to start tracking when VS Code, PhpStorm, or Xcode launches.
- **Automation Ready**: URL schemes for macOS Shortcuts, Raycast, Alfred, and shell scripts.
- **Native**: Built with Swift 6 and SwiftUI, zero external dependencies.
- **Localization**: English and German interface.

## Installation

> [!IMPORTANT]
> Releases of this fork are built without an Apple Developer account: they are ad-hoc signed and **not notarized by Apple**. The Homebrew cask removes the macOS quarantine flag for you; for a manual install see below. Requires a Mac with Apple Silicon.

### Homebrew (Recommended)

```bash
brew install --cask clemens-code/tap/kimai-clock
```

Update, remove, or drop the tap:

```bash
brew upgrade --cask kimai-clock
brew uninstall --cask kimai-clock
brew untap clemens-code/tap
```

### Direct Download

1. Download `KimaiClock.dmg` from the latest [release](../../releases/latest).
2. Open the disk image and drag **KimaiClock.app** into your **Applications** folder.
3. Allow the first launch (the app is not notarized, so macOS would otherwise block it):
   ```bash
   xattr -dr com.apple.quarantine /Applications/KimaiClock.app
   ```
4. Open KimaiClock and continue with [Getting Started](#getting-started).

### Switching from the original KimaiClock

Both apps share the same bundle identifier, so only one of them can be installed. Quit and remove the original first:

```bash
brew uninstall --cask foraum-gmbh/foraum/kimai-clock
brew untap foraum-gmbh/foraum
```

Settings are not carried over: enter your server URL and API token again.

## Getting Started

### 1. Create an API token in Kimai

KimaiClock signs in with a personal API token, not with your password.

1. Log in to your Kimai web interface.
2. Open your **user profile** (avatar in the top right) and go to **API Access**.
3. Create a new token. Give it a name such as `KimaiClock`; an expiry date is optional.
4. **Copy the token right away** — Kimai shows it exactly once. If you lose it, delete it and create a new one.

Treat the token like a password: it grants access to your timesheets. KimaiClock stores it in the app's local settings on your Mac.

### 2. Connect KimaiClock

1. Click the KimaiClock icon in the menu bar to open the popover.
2. Expand **Settings**.
3. **Server URL**: your Kimai address, e.g. `kimai.example.com`. `https://` is added automatically; enter `http://…` explicitly only for a server without TLS (a warning is shown).
4. **User API Token**: paste the token from step 1.
5. Check the **Server:** line at the bottom of Settings. It shows your Kimai version when the connection works; `Server: unsupported` means the server is older than Kimai 2, `Server: 500` means the URL or token is wrong.

### 3. Track time

1. Expand **Activities** and type part of a project name into **Search Projects**.
2. Click the project, then choose the activity from the drop-down below it. The header shows `project / activity`.
3. Optional: enter a description (**Add description…**) and choose **Tags**.
4. Press **▶** to start. Press **⏸** to pause and **⏹** to stop; both save the timesheet in Kimai.
5. Next time, start from **Recent Activities** with one click.

While paused, you can still change the description and tags; they apply when you resume. Right-click the play or stop button to **edit the description** of the running timer.

### 4. Optional settings

| Setting | What it does |
|---|---|
| **Fetch active server timer** | When to check Kimai for a timer started elsewhere: on opening the popover, or every 5, 15 or 30 minutes. |
| **Autostart** | *Launch KimaiClock on login*. |
| **Idle reminder after** | Minutes of inactivity before the idle dialog appears. Leave empty to disable. |
| **Reset all Notices** | Show dialogs again that you dismissed with "Don't Show Again" / "Remember my choice". |

> [!NOTE]
> On quit, shutdown and sleep, KimaiClock stops the running timesheet on the server.

## Controls and Shortcuts

### Menu Bar Status Item

- **Left click**: Open or close the popover.
- **Left click and hold**: Open the Kimai web interface in your default browser.
- **Right click**: Toggle play/pause for the active task.

### Stop Button Context Menu

Right-click the stop button inside the popover for more options:

| Action | Description |
|---|---|
| **Edit description** | Changes the note of the current timesheet. |
| **Stop with description** | Prompts for a note, then stops and records the timesheet with that comment. |
| **Discard & delete** | Cancels the timer and removes the timesheet from the server (only while the timer is running). |

### Idle Detection

KimaiClock monitors system idle time while an activity is running. When you return after reaching your configured idle threshold, a dialog lets you resolve the gap:

- **Continue without idle time**: Keep the timer running and deduct the absence from the timesheet.
- **Continue and keep idle time**: Keep the timer running and count the absence (e.g. a phone call).
- **Stop**: Stop and record the timesheet at the moment you stepped away.

Tick **Remember my choice** to apply the same option automatically next time; **Reset all Notices** in Settings brings the dialog back.

When your Mac goes to sleep, the running timesheet is stopped at the moment of sleep, just like on quit or shutdown.

## URL Schemes and Automation

KimaiClock registers URL schemes for hotkeys and automated workflows:

| URL | Description |
|---|---|
| `kimai-clock://startLast` | Starts the most recently tracked activity. |
| `kimai-clock://startLast?description=...` | Starts the most recent activity with a timesheet description. |
| `kimai-clock://pause` | Pauses the running timer. |
| `kimai-clock://stop` | Stops and records the active timesheet. |

### Examples

**Terminal**
```bash
# Start the most recent activity with a note
open "kimai-clock://startLast?description=Reviewing%20pull%20requests"

# Pause the active timer
open "kimai-clock://pause"
```

**macOS Shortcuts**
1. Open the **Shortcuts** app and create a new shortcut.
2. Add the **Open URLs** action.
3. Enter the URL (such as `kimai-clock://startLast`).
4. Open the shortcut details and select **Add Keyboard Shortcut** (such as `Command + Shift + L`).

These URLs also work from launchers such as **Raycast**, **Alfred**, or **BetterTouchTool**.

## Widget

KimaiClock includes a widget extension for the desktop and Notification Center that shows the active activity and running duration. It may not work in the unsigned releases of this fork, because widgets need a sandboxed build signed with an Apple Developer account.

## Development

Requires full Xcode 26 (not just the Command Line Tools). No dependencies to install.

```bash
# Build and run the unit tests (same as CI)
xcodebuild -project KimaiClock.xcodeproj -scheme KimaiClock -destination "platform=macOS" CODE_SIGNING_ALLOWED=NO test

# Lint (same as CI)
swiftlint --strict
```

`CODE_SIGNING_ALLOWED=NO` skips signing, which is set up for the original project's Apple team. To try a local build, quit any running KimaiClock first — a second instance terminates itself:

```bash
pkill -x KimaiClock; xcodebuild -project KimaiClock.xcodeproj -scheme KimaiClock -configuration Debug -destination "platform=macOS" -derivedDataPath build CODE_SIGNING_ALLOWED=NO build -quiet && open build/Build/Products/Debug/KimaiClock.app
```

SwiftUI views are not unit tested. Behaviour that needs a real Mac, such as sleep, idle detection and the menu bar, is tested manually.

### Releasing

Releases are tag-driven. Pushing a tag `vX.Y.Z` (strict semver, greater than the latest release) makes CI build the DMG, publish a GitHub release, commit the version bump to `main`, and update the cask in [clemens-code/homebrew-tap](https://github.com/clemens-code/homebrew-tap):

```bash
git tag v1.7.0 && git push origin v1.7.0
```

The workflow needs the repository secret `HOMEBREW_TAP_TOKEN`: a fine-grained token with *Contents: read & write* on the tap repository only.

## Changelog

Changes up to the fork are listed in [CHANGELOG.md](./CHANGELOG.md); later changes are in this repository's [releases](../../releases).

## Contributors

Original project by [Foraum GmbH](https://github.com/Foraum-GmbH/kimai-clock) and its contributors:

<a href="https://github.com/undeadd"><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/8116188&w=300&h=300&fit=cover&mask=circle" width="50" height="50" style="border-radius:50%"/></a>
<a href="https://github.com/fabian-rohr"><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/20979750&w=300&h=300&fit=cover&mask=circle" width="50" height="50" style="border-radius:50%"/></a>
<a href="https://github.com/dmytro-kerest"><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/118644&w=300&h=300&fit=cover&mask=circle" width="50" height="50" style="border-radius:50%"/></a>
<a href="https://claude.ai"><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/81847&w=300&h=300&fit=cover&mask=circle" width="50" height="50" style="border-radius:50%"/></a>

Fork maintained by [clemens-code](https://github.com/clemens-code). Issues and feature requests are welcome on the [issues page](../../issues).

## License

Distributed under the MIT License. See [LICENSE](./LICENSE.md) for details.
