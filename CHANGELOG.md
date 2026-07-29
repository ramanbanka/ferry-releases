# Changelog

Notable changes, newest first. Follows [Keep a Changelog](https://keepachangelog.com)
loosely and [semantic versioning](https://semver.org).

## Unreleased

## 0.1.1

Fixes data loss on a common path, and reads are faster.

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

### Changed

- Reads are about 17% faster — 21 to 25 MB/s on the phone this was measured against.
  Two concurrency limits were set below what the transport sustains.
- Mount directories use underscores rather than spaces, so paths can be typed in a
  terminal without quoting: `~/Ferry/SM_G781B_SD_card`. Notifications and Finder's
  sidebar are unchanged.

### Internal

- The conformance suite covers the write path — WRITE, CREATE, MKDIR, REMOVE, RMDIR,
  RENAME and COMMIT had no automated coverage, and the RENAME data loss above was
  found by adding it. 31 checks, up from 18.
- A read-throughput floor, since the speed regression above went unnoticed for want of
  anything measuring it.

## 0.1.0

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
