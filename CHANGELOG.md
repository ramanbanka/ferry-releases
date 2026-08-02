# Changelog

All notable changes to Ferry are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.4.1] - 2026-08-02

Wi-Fi fixes.

### Added

- `ferry status` shows which version is running.

### Fixed

- A phone that has left the network no longer keeps asking to be paired.
- A Wi-Fi mount no longer tells you to reconnect a cable you do not have.
- `ferry pair` no longer downgrades a phone that is already paired.
- A file could be reported as saved before it reached the phone.
- Some failures made Finder wait instead of showing an error.
- A mount that fails no longer leaves an empty folder in `~/Ferry`.

## [0.4.0] - 2026-08-01

### Added

- **Now you have an option to browse your phone over Wi-Fi.** Run `ferry pair` in Terminal
  once and it appears in Finder whenever it is on the same network as your Mac — no cable,
  nothing plugged in.

  Note: Wi-Fi can be up to 9x slower than a cable. 4K video played straight from the phone
  may buffer.

- **Plug in using USB and mounts speed up automatically.** Unplug and they go back to Wi-Fi.

- **`ferry status` says which one you are on.**

### Fixed

- **Long sessions stay tidier.** Connecting and disconnecting repeatedly no longer leaves
  anything behind.

## [0.3.0] - 2026-07-30

Browsing is faster.

### Changed

- **Photo folders scroll better.** Pause for a moment and the rest of the folder is
  ready before you reach it — a second screenful that took about 115 ms per photo now
  takes about 3 ms. Going back to a folder you were in earlier is immediate.

- **Thumbnails load about 25% faster and video plays about 15% faster**, throughout.

### Fixed

- **Watching a video no longer makes the surrounding folder slow again.**

- **Re-reading part of a file no longer costs you previews** you had already loaded.

## [0.2.1] - 2026-07-29

Fixes a Homebrew install that left Ferry unable to find adb or post its own
notifications. **Anyone who installed 0.2.0 with Homebrew should upgrade.**

### Fixed

- **Homebrew installs could not find adb or terminal-notifier.** Ferry records where its
  helpers live when it registers with launchd, since a background agent gets no shell
  environment. It looked them up on `PATH` — but Homebrew does not run a cask's install
  step with your `PATH`; it substitutes its own. So nothing was found, and the agent
  started with only the system directories: no adb server could be started, and
  notifications fell back to being branded "Script Editor".

  Ferry now asks Homebrew where its programs are rather than trusting `PATH`, and looks
  again at runtime, so a helper installed *after* Ferry is picked up without reinstalling.

  Only Homebrew installs were affected. `ferry enable` run by hand from a terminal
  worked, which is why this was not caught before release.

## [0.2.0] - 2026-07-29

The commands changed. Ferry now explains itself when a phone will not mount.

### Upgrading from 0.1.1

`brew upgrade --cask ferry` does not work from 0.1.1, because the upgrade runs the
old version's uninstall step and that version does not have the command the cask now
calls. Remove it first:

```bash
brew uninstall --cask ferry
brew install --cask ferry
```

### Added

- When a phone cannot be mounted, Ferry says why: USB debugging switched off, the
  phone not yet allowing this Mac, adb not installed, or the phone not responding.
  Each explains what to do, and the message closes itself once the problem is
  resolved or the phone is unplugged.

### Changed

- **New commands.** `install`, `uninstall`, `devices` and `agent` are gone.

  | Was | Now |
  |---|---|
  | `ferry install` | `ferry enable` — register with launchd and start |
  | `ferry uninstall` | `ferry disable` — stop and deregister |
  | `ferry devices` | `ferry status`, which already listed phones |
  | `ferry agent` | `ferry run`, and it is no longer advertised |

  New: `ferry start`, `ferry stop` and `ferry restart` act on Ferry now without
  changing whether it runs at login, and `ferry log` follows the log.

- **`ferry stop` now works.** The launchd job used to restart the agent on any exit,
  so there was no way to stop it. A deliberate stop is now distinguished from a
  crash: stopping stays stopped, crashing is still recovered.

### Fixed

- Notifications were attributed to "Script Editor" when `terminal-notifier` was
  installed somewhere unexpected.
- A phone with a memory card logged every connect and disconnect twice, and named
  itself by serial number in some messages and by model in others.
- A phone missing for a single moment — a jostled cable — caused both volumes to
  unmount and remount, with four notifications.
- Browsing a folder wrote a log line for every file in it, because Finder reads each
  one to draw a thumbnail. The log records what Ferry did, not which files were
  opened.
- An unplugged phone was logged as an error. It is expected, and Ferry recovers.
- The log is timestamped. It was not, unless Ferry was run by hand with `-v`.

## [0.1.1] - 2026-07-29

Fixes data loss on a common path, and reads are faster.

### Changed

- Reads are about 17% faster — 21 to 25 MB/s on the phone this was measured against.
  Two concurrency limits were set below what the transport sustains.
- Mount directories use underscores rather than spaces, so paths can be typed in a
  terminal without quoting: `~/Ferry/SM_G781B_SD_card`. Notifications and Finder's
  sidebar are unchanged.

### Fixed

- **Renaming a file could lose its contents.** Writes are buffered until a file is
  closed, and a rename moved the file without flushing them first — so the move
  carried an empty file and the buffered data was discarded. This was reachable by
  ordinary use: apps save by writing to a temporary name and renaming it into place,
  so editing a file on the phone and saving could lose the edit.
- **Truncating a file did nothing.** Shortening a file left it at its original length
  and reported success, so anything relying on truncation silently got the wrong
  result. Shrinking and growing both work now, and growing zero-fills.
- Deleting a file that did not exist reported success instead of "no such file".
- A phone whose internal storage is not at `/storage/emulated/0` could not be mounted
  at all; `/sdcard` is now used as a fallback.
- A phone that cannot be mounted now says so, once, instead of retrying silently
  forever while nothing appears in Finder.

## 0.1.0 - 2026-07-28

First release.

- Phones mount automatically when plugged in, via a launchd user agent. No kernel
  extension, no root, no SIP changes.
- Full read/write: browse, copy both directions, move, rename, delete, edit in place.
- Video streams and seeks rather than being copied first.
- Unplugging is handled — operations fail cleanly and the mount recovers when the
  phone returns, with no remount.
- Several phones can be mounted at once, each named after its model.
- A memory card appears as its own volume alongside internal storage, with its own
  free space. Cards inserted while the phone stays plugged in are picked up.
- Notifications on connect and disconnect, and when unsaved data is lost.
- Finder's eject button works and flushes pending writes.
- Installs with `brew install --cask ferry`, which pulls in dependencies and starts
  the agent.


[0.4.1]: https://github.com/ramanbanka/ferry-releases/compare/v0.4.0...v0.4.1
[0.4.0]: https://github.com/ramanbanka/ferry-releases/compare/v0.3.0...v0.4.0
[0.3.0]: https://github.com/ramanbanka/ferry-releases/compare/v0.2.1...v0.3.0
[0.2.1]: https://github.com/ramanbanka/ferry-releases/compare/v0.2.0...v0.2.1
[0.2.0]: https://github.com/ramanbanka/ferry-releases/compare/v0.1.1...v0.2.0
[0.1.1]: https://github.com/ramanbanka/ferry-releases/releases/tag/v0.1.1
