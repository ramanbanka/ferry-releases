# Ferry

**Mount an Android phone on your Mac. Browse it in Finder, like a USB drive.**

![macOS 11+](https://img.shields.io/badge/macOS-11%2B-blue)
![Apple Silicon](https://img.shields.io/badge/Apple%20Silicon-arm64-lightgrey)
![Version](https://img.shields.io/github/v/release/ramanbanka/ferry-releases?color=green&label=version)
![Free](https://img.shields.io/badge/price-free-brightgreen)

**[ramanbanka.github.io/ferry](https://ramanbanka.github.io/ferry/)** — what it does, how
it compares, and how to install it.

Plug an Android phone into your Mac with a USB cable and it appears in Finder as a
volume. Drag files in and out, open photos, play videos, edit a document in place.
A memory card shows up as its own volume next to it.

You also have an option to browse it over Wi-Fi, with no cable at all — see
[Without a cable](#without-a-cable).

```bash
brew tap ramanbanka/ferry
brew trust ramanbanka/ferry
brew install --cask ferry
```

That's the whole setup. Ferry runs quietly in the background from then on: plug in a
phone, it appears; unplug it, it goes away.

---

## Everything you'd expect from a drive

| | |
|---|---|
| **Browse** | Open your phone in Finder. Icon view, list view, column view, sort by size or date — all of it. |
| **Copy and paste** | ⌘C on your Mac, ⌘V on the phone. Or drag. Both directions. |
| **Move and rename** | Drag files between folders on the phone. Rename in place. |
| **Delete** | ⌘⌫ works. Files go from the phone. |
| **Open in place** | Double-click a file and it opens in whatever app you normally use. Nothing is downloaded first. |
| **Play video** | Open a 4K clip in your player of choice and scrub through it. |
| **Edit and save** | Open a text file or document, edit, ⌘S. Saves back to the phone. |
| **Quick Look** | Select a file, hit space. Works. |
| **Search in a folder** | Finder's search within a folder works, as do `find` and `grep`. |
| **Use it from the terminal** | `cp`, `rsync`, `ls`, `find`, `grep` all work. |
| **SD cards** | A memory card appears as its own volume beside internal storage, with its own free space. |
| **Multiple phones** | Plug in two, get two sets of volumes, each named after its phone. |
| **Eject** | The eject button in Finder's sidebar works, and flushes anything still being written. |
| **No cable** | Pair once and it mounts over Wi-Fi, from anywhere on the network. |

## How it compares

| | **Ferry** | **Android File Transfer** | **OpenMTP** | **MacDroid** |
|---|---|---|---|---|
| **Cost** | Free | Free | Free | Paid tier |
| **Where files appear** | **A volume in Finder** | Its own window | Its own window | A mounted drive |
| **Works in every app** | **Yes** — Open/Save dialogs, Preview, terminal | No, copy out first | No, copy out first | Yes |
| **Transport** | ADB | MTP | MTP | ADB / MTP |
| **SD card as its own volume** | **Yes**, with its own free space | No | No | Not stated |
| **Runs in the background** | **Yes** — plug in and it mounts | No, launch the app | No, launch the app | Yes |
| **Works without a cable** | **Yes**, over Wi-Fi | No | No | Not stated |

Rows come from each project's own documentation. The row that matters is the second one. Ferry makes your phone a *volume*, so a 4K video
opens in your player, a document opens in Word and saves back, and `rsync` works. Tools
with their own window make you copy a file out, edit it, and copy it back.

## Why you might prefer it

**Nothing is copied.** Your data stays on the phone until you choose to copy it. Nothing is
uploaded, synced, or sent to anyone else — no cloud, no account. Over Wi-Fi it goes between
your phone and your Mac and nowhere else.

**It's fast.** 25 MB/s reads over USB. 4K video streams and scrubs without stuttering.

**It survives the cable.** Unplug mid-session and nothing hangs. Plug back in and it picks up
where it was — and if the phone is paired for Wi-Fi, the volumes stay up without it.

**It gets out of the way.** No window, no dock icon, no menu bar item — just a
notification when a phone connects.

**Nothing to trust it with.** No kernel extension, no root password, no disabling any
of your Mac's security. Free to use, with no account and no sign-up.

## Speed

Over USB. Wi-Fi is slower — see [Without a cable](#without-a-cable).

| | |
|---|---|
| Reading a file | **25 MB/s** — an 80 MB video in about three seconds |
| Jumping to a point in a video | under 50 ms, so scrubbing feels immediate |
| Copying a photo to your Mac | about half a second |
| Opening a folder of 1,500 photos | under a second |
| Re-opening a file you just read | instant, served from memory |

## Before you start

- **A Mac with Apple Silicon, running macOS 11 or newer.**
- **An Android phone with USB debugging on** — a one-time setup:
  1. Settings → About phone → tap **Build number** seven times
  2. Settings → Developer options → **USB debugging** → on
  3. Plug in, unlock the phone, tap **Allow** on the prompt
- **A USB cable.** For Wi-Fi instead, see [Without a cable](#without-a-cable) — you still
  need the steps above, plus Android 11 or newer.

## Install

### With Homebrew

```bash
brew tap ramanbanka/ferry
brew trust ramanbanka/ferry
brew install --cask ferry
```

Homebrew installs everything Ferry needs and starts it. Nothing else to do — plug in a
phone and open Finder.

To update later: `brew upgrade --cask ferry`. To remove it:
`brew uninstall --cask ferry`.

### From a downloaded release

Download `ferry-<version>-macos-arm64.tar.gz` from
[Releases](../../releases), then in Terminal:

```bash
cd ~/Downloads
tar -xzf ferry-*-macos-arm64.tar.gz
sudo rm -f /usr/local/bin/ferry
sudo install -m 755 ferry-*-macos-arm64/ferry /usr/local/bin/ferry
xattr -d com.apple.quarantine /usr/local/bin/ferry 2>/dev/null || true
ferry enable
```

macOS tags anything downloaded in a browser and won't run it until the tag is cleared —
that's the `xattr` line. `ferry enable` is what registers the agent; `ferry start` only
starts one that is already registered, so it will not work on a fresh install.

You also need `adb`, which Homebrew would have installed for you:

```bash
brew install --cask android-platform-tools
brew install terminal-notifier
```

## Using it

Phones mount at `~/Ferry/<PhoneModel>` and appear in Finder's sidebar. Mostly there is
nothing to do, but:

```bash
ferry status     # what's mounted, and over USB or Wi-Fi
ferry pair       # set the phone up to mount over Wi-Fi, no cable
ferry log        # follow what Ferry is doing
ferry restart    # if something seems stuck
ferry stop       # stop until your next login
```

`ferry --help` lists the rest.

## Without a cable

You also have an option to browse your phone over Wi-Fi. Pair it once and it appears in
Finder whenever it is on the same network as your Mac — no cable, nothing plugged in.

**Note: Wi-Fi can be up to 9x slower than a cable. 4K video played straight from the phone
may buffer.**

On the phone — this needs Android 11 or newer:

1. Settings → Developer options → **Wireless debugging**, and turn it on
2. Tap the words *Wireless debugging* (not the switch), then **Pair device with pairing code**

Leave that screen open, and on your Mac:

```bash
ferry pair
```

Type the six digits the phone shows. That is the whole setup — Ferry finds the phone by
itself from then on.

Plug in using USB and mounts speed up automatically; unplug and they go back to Wi-Fi.

If the phone stops accepting your Mac, wireless debugging was probably turned off and on
again, which makes the phone forget it. Run `ferry pair` in Terminal again.

## If something goes wrong

| Problem | What to do |
|---|---|
| Phone doesn't appear in Finder | Unlock it and check for an "Allow USB debugging?" prompt. Then `ferry status`. |
| `ferry status` shows no phones | Try a different cable — many USB-C cables only carry power. Over Wi-Fi, check the phone is awake and on the same network. |
| Worked before, not now | Another Android transfer app may have taken the connection. Quit it, then `ferry status`. |
| Transfers feel slow | Check `ferry status` — over Wi-Fi that is expected, and plugging in speeds it up. On a cable, use USB 3; USB 2 caps at about 25 MB/s. |
| Video buffers or stutters | Almost certainly Wi-Fi. Plug the cable in and it keeps playing. |
| Phone won't pair, or stopped accepting this Mac | Wireless debugging was probably turned off and on again. Run `ferry pair` in Terminal again. |
| "Refusing to load cask … untrusted tap" | Run `brew trust ramanbanka/ferry`, then install again. |
| `Killed: 9` when running `ferry` | macOS quarantined the download. `xattr -d com.apple.quarantine $(which ferry)`. |
| Eject did nothing | Something still has a file open. Close it, or unplug the phone. |

## What's new

[CHANGELOG.md](CHANGELOG.md) lists what changed in each version.

## Bug reports

Welcome — [open an issue](../../issues). Please include your phone's model name and the
output of `ferry status`.

## Licence

Ferry is free to use, and stays that way. It is not open source — full terms in
[LICENSE](LICENSE), included alongside this file.
