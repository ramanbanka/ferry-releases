# Ferry

**Your Android phone, in Finder. Over a cable or Wi-Fi.**

![macOS 11+](https://img.shields.io/badge/macOS-11%2B-blue)
![Apple Silicon](https://img.shields.io/badge/Apple%20Silicon-arm64-lightgrey)
![Version](https://img.shields.io/github/v/release/ramanbanka/ferry-releases?color=green&label=version)
![Free](https://img.shields.io/badge/price-free-brightgreen)

Plug in your phone, or just be on the same Wi-Fi, and it shows up in Finder like a USB drive.
Browse photos, watch videos, and drag files both ways. No app to open, nothing to click.
Ferry just works in the background.

```bash
brew tap ramanbanka/ferry
brew trust ramanbanka/ferry
brew install --cask ferry
```

More at **[ramanbanka.github.io/ferry](https://ramanbanka.github.io/ferry/)**.

---

## What you can do

| | |
|---|---|
| **Browse** | Photos, videos and files, right in Finder |
| **Open in place** | Double-click anything. No copying first |
| **Copy both ways** | Drag and drop, fast and safe |
| **Edit and save** | Save straight back to the phone |
| **Wi-Fi** | Pair once by QR code. Move between cable and Wi-Fi without noticing |
| **SD card** | Shows up as its own drive |
| **Terminal** | Works with `cp`, `rsync` and friends |

## Before you start

- A Mac with Apple Silicon, on macOS 11 or newer.
- On the phone, once:
  1. **Settings → About phone**, tap **Build number** seven times.
  2. **Settings → Developer options → USB debugging**, on.
- A cable that carries data. Many USB-C cables only charge.

## Install

### With Homebrew (recommended)

```bash
brew tap ramanbanka/ferry
brew trust ramanbanka/ferry
brew install --cask ferry
```

Then plug in the phone, unlock it, and tap **Allow** on "Allow USB debugging?". It appears in
Finder.

Update with `brew upgrade --cask ferry`. Remove with `brew uninstall --cask ferry`.

### From a download

Get `ferry-<version>-macos-arm64.tar.gz` from [Releases](../../releases), then:

```bash
brew install --cask android-platform-tools     # adb, which Ferry needs
brew install terminal-notifier                 # for notifications
cd ~/Downloads
tar -xzf ferry-*-macos-arm64.tar.gz
sudo install -m 755 ferry-*-macos-arm64/ferry /usr/local/bin/ferry
xattr -d com.apple.quarantine /usr/local/bin/ferry 2>/dev/null || true
ferry enable
```

The `xattr` line clears the flag macOS puts on downloads. Without it, macOS stops the program
from running.

## Without a cable

Needs Android 11 or newer.

**Used the cable before?** Just turn on **Settings → Developer options → Wireless debugging**.
The phone then shows up over Wi-Fi too.

**Never used the cable?** Pair once by QR code:

1. On the phone: **Settings → Developer options → Wireless debugging**, on. Tap the words
   **Wireless debugging** (not the switch) and leave that screen open.
2. On the Mac, run `ferry pair` and choose **Pair a new device**.
3. On the phone, tap **Pair device with QR code** and point it at the code on your screen.

That's all. From then on the phone appears whenever it is on the same Wi-Fi as your Mac.

## Commands

You rarely need these.

```bash
ferry status     # what is connected and mounted, over cable or Wi-Fi
ferry pair       # set a phone up for Wi-Fi
ferry log        # follow what Ferry is doing
ferry restart    # if something seems stuck
ferry stop       # stop until you log in again
ferry disable    # stop, and don't start at login
```

## If something goes wrong

| Problem | What to do |
|---|---|
| The phone doesn't appear | Unlock it and look for "Allow USB debugging?". Then run `ferry status`. |
| `ferry status` shows no phone | Try another cable. Over Wi-Fi, check the phone is awake and on the same network. |
| It worked before, not now | Another Android transfer app may have taken the phone. Quit it. |
| The phone stopped accepting this Mac over Wi-Fi | Plug in the cable once and tap **Allow**, or run `ferry pair` again. |
| Eject does nothing | Something still has a file open. Close it, or unplug the phone. |
| "Refusing to load cask … untrusted tap" | Run `brew trust ramanbanka/ferry`, then install again. |
| `Killed: 9` when running `ferry` | Run `xattr -d com.apple.quarantine $(which ferry)`. |

Found a bug? [Open an issue](../../issues) with your phone's model and the output of
`ferry status`.

## Privacy

Your files go between your phone and your Mac, and nowhere else. No account, no cloud, no
driver.

## Support

Ferry is free. Feel free to support me at [ko-fi.com/ramanbanka](https://ko-fi.com/ramanbanka).

## What's new

See [CHANGELOG.md](CHANGELOG.md).

## Licence

Free to use; not open source. Full terms in [LICENSE](LICENSE).

The Android robot in Ferry's icon is reproduced or modified from work created and shared by
Google and used according to terms described in the
[Creative Commons 3.0 Attribution License](https://creativecommons.org/licenses/by/3.0/).
Android is a trademark of Google LLC; Mac is a trademark of Apple Inc. Ferry is not affiliated
with either.
