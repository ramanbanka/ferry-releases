---
name: Phone doesn't work properly
about: Your phone doesn't appear, or files look wrong
labels: device
---

**Phone model**
<!-- e.g. Pixel 8, Galaxy S21, Redmi Note 12. This is the single most useful thing
     you can tell us — Android vendors lay out storage differently, and that is
     where most device-specific bugs come from. -->

**Android version**

**macOS version**
<!-- Apple menu > About This Mac -->

**What happened**
<!-- What you expected, and what happened instead. -->

**Does the phone show up at all?**

```
ferry status
```

<!-- paste the output -->

**Log**

```
ferry log -n 50 --no-follow
```

<!-- paste the output; if the file is empty or missing, say so -->

**Anything unusual about the setup?**
<!-- USB hub, dock, adapter, adb over Wi-Fi, more than one phone attached, SD card,
     unusual storage layout. -->
