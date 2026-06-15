<!-- <p align="center">
  <img src="[insert XXX IMAGE URL]" alt="Image" width="120">
</p> -->

 
<h1 align="center">Linux Troubleshooting Tale with AIs - 01</h1>
<h3 align="center"> When AI wants me to remove packages - The Case of the Stubborn Library</h3>

<!-- <p align="center">
  Enter Text <br>
  <a href="#">Acer Nitro 5 + Kubuntu 26.04</a> |
  <a href="#">MBP 2019 + Kubuntu-T2</a> |
  <a href="#">MBP 2011 + MX-23/25_KDE</a> |
  <a href="#other-references">References</a>
</p>
 -->

---
## Chapter 1: The Suspect is Identified

It started innocently enough. A simple question: *"Is ibus still installed?"*

One command revealed the truth:

```bash
dpkg -l | grep ibus

## terminal output: 
ii  ibus-data               1.5.34~rc2-1      all       Intelligent Input Bus - data files 
ii  ibus-gtk3:amd64         1.5.34~rc2-1      amd64     Intelligent Input Bus - GTK3 support 
ii  ibus-gtk4:amd64         1.5.34~rc2-1      amd64     Intelligent Input Bus - GTK4 support 
ii  libgusb2a:amd64         0.4.9-7           amd64     GLib wrapper around libusb1 
ii  libibus-1.0-5:amd64     1.5.34~rc2-1      amd64     Intelligent Input Bus - shared library 
ii  libusb-1.0-0:amd64      2:1.0.29-2build1  amd64     userspace USB programming library 
ii  libusbmuxd-2.0-7:amd64  2.1.1-1           amd64     Client library to handle usbmux connections with iOS devices 

```

And there it was — not one, not two, but **multiple `ii` entries** staring back at the screen. `ii` in Debian-land means *fully installed*. IBus hadn't gone anywhere.

---

## Chapter 2: The Purge

Armed with confidence, the order was given:

```bash
sudo apt purge ibus ibus-data ibus-gtk3 ibus-gtk4 libibus-1.0-5 --autoremove
```

*Clean sweep*, or so it seemed. But when the dust settled and the dpkg list was checked again, **four lines** came back instead of three. One stubborn survivor remained:

```bash
# terminal outout
ii  libibus-1.0-5:amd64    1.5.34~rc2-1    Intelligent Input Bus - shared library
```

`--autoremove` had left it behind. Suspicious.

---

## Chapter 3: The Hasty Recommendation

*"Remove it manually!"* came the advice, quick and confident:

```bash
sudo apt purge libibus-1.0-5 --autoremove
```

But the user paused. *Wait — why didn't autoremove pick this up in the first place? Could something else need it?*

A wise instinct.

---

## Chapter 4: The Investigation

Before pulling the trigger, the right question was asked first:

```bash
apt rdepends --installed libibus-1.0-5
```

The answer came back immediately:

```
libibus-1.0-5
Reverse Depends:
  Depends: plasma-desktop (>= 1.5.1)
```

**`plasma-desktop`.** The heart of the KDE desktop itself was holding onto this library. Removing `libibus-1.0-5` could have taken the entire desktop environment down with it.

The hasty command was never run. Disaster averted.

---

## Chapter 5: The Lesson

The AI had given a `purge` command without first checking what depended on the library. The user's instinct to question it — *"what if removing it breaks something?"* — was exactly right.

The takeaway:

- **`apt rdepends --installed <package>`** before removing any library
- **`sudo apt -s purge <package>`** for a safe dry run that shows what *would* happen
- **`sudo` commands deserve a pause** — understand before you execute
- **AI doesn't know your exact system state** — use it as a guide, not gospel

---

## Epilogue

`libibus-1.0-5` remains on the system, quietly doing its job for `plasma-desktop`. The ibus daemon is not running. No harm done.

And somewhere, a KDE desktop continues to function — because someone thought to ask *"but why?"* before blindly trusting a command.

---
*The system is now ready for the next chapter: installing **Fcitx5**.*
