# Fcitx5 + Zhuyin Installation Guide
### Kubuntu 26 / KDE Plasma / Wayland

---

> ### 💡 Chewing vs Zhuyin — What's the Difference?
> These two terms refer to the **same thing** but from different angles:
> - **Zhuyin** (注音) / **Bopomofo** — the *input method system* you type with (ㄅㄆㄇㄈ...)
> - **Chewing** — the name of the *software engine* that implements Zhuyin
>
> So `fcitx5-chewing` is the correct package to install for Zhuyin input. It just happens to be named after the engine rather than the input method. In `fcitx5-configtool` it appears as "Chewing" — that's your Zhuyin! ✅

---

## Step 1 — Check What's Already Installed

Before doing anything, verify the current state of ibus and fcitx5.

```bash
# Check ibus
dpkg -l | grep ibus

# Check fcitx5
apt list --installed | grep fcitx5

# Check if any input method daemon is running
ps aux | grep ibus
ps aux | grep fcitx
```

---

## Step 2 — Remove IBus Completely

> ⚠️ **Important:** Before removing any library, always check if something depends on it first!

```bash
# Check what depends on the ibus shared library BEFORE removing
apt rdepends --installed libibus-1.0-5
```

If `plasma-desktop` depends on `libibus-1.0-5`, **do NOT remove it** — it will break your desktop. Leave that library in place.

Remove only the ibus packages that are safe:

```bash
sudo apt purge ibus ibus-data ibus-gtk3 ibus-gtk4 --autoremove
```

Clean up leftover config:

```bash
rm -rf ~/.config/ibus
```

Verify ibus is gone (expect only USB-related `libgusb`/`libusb`/`libusbmuxd` lines to remain):

```bash
dpkg -l | grep ibus
```

---

## Step 3 — Remove Previous Fcitx5 Installation (Clean Slate)

```bash
sudo apt purge fcitx5 fcitx5-* --autoremove
rm -rf ~/.config/fcitx5
```

Verify it's clean:

```bash
apt list --installed | grep fcitx5
```

---

## Step 4 — Clean Up Environment Variables

Check if any old input method variables exist:

```bash
grep -r "IM_MODULE\|XMODIFIERS" ~/.profile ~/.bashrc ~/.xprofile ~/.bash_profile 2>/dev/null
```

If you see any `GTK_IM_MODULE`, `QT_IM_MODULE`, or `XMODIFIERS` lines, remove them — they are **not needed on Wayland** and will cause warnings.

```bash
nano ~/.profile
```

Remove these lines if present:
```bash
export GTK_IM_MODULE=fcitx   # remove
export QT_IM_MODULE=fcitx    # remove
export XMODIFIERS=@im=fcitx  # remove
```

> 💡 On Wayland, the input method protocol handles this natively — no env vars needed.

---

## Step 5 — Install Fcitx5

```bash
sudo apt update
sudo apt install fcitx5 fcitx5-chinese-addons fcitx5-config-qt fcitx5-frontend-gtk3 fcitx5-frontend-qt5 fcitx5-chewing
```

**What each package does:**

| Package | Purpose |
|---|---|
| `fcitx5` | Main input method daemon |
| `fcitx5-chinese-addons` | Bundle of Chinese input engines (Pinyin, Cangjie, etc.) |
| `fcitx5-config-qt` | Configuration UI tool |
| `fcitx5-frontend-gtk3` | Input support for GTK3 apps (Firefox, etc.) |
| `fcitx5-frontend-qt5` | Input support for Qt5 apps (Kate, Dolphin, etc.) |
| `fcitx5-chewing` | Zhuyin/Bopomofo 注音 input engine ("Chewing" is the engine name, Zhuyin is what you type) |

> 💡 `fcitx5-frontend-qt6` will likely be installed automatically as a dependency.

---

## Step 6 — Configure KDE to Launch Fcitx5 via Wayland

> ⚠️ On Wayland, **do NOT** start `fcitx5` manually. KWin must launch it.

1. Open **System Settings**
2. Search for **"Virtual Keyboard"**
3. Select **"Fcitx 5"** *(not the Experimental Wayland version)*
4. Click **Apply**

---

## Step 7 — Check im-config (Do Not Skip!)

```bash
# Check available options
im-config -l

# Check current setting
cat ~/.config/im-config/config 2>/dev/null || echo "No im-config setting found"
```

If `fcitx5` is already first in the list and no config file exists, **no changes needed**.

If needed, set it:
```bash
im-config -n fcitx5
```

---

## Step 8 — Log Out and Back In

Log out completely and log back in. KWin will now launch Fcitx5 automatically.

Verify Fcitx5 is running:
```bash
ps aux | grep fcitx
```

You should see `/usr/bin/fcitx5` in the output.

---

## Step 9 — Add Zhuyin (Chewing) Input Method

> 💡 You're searching for **"Chewing"** — that's the engine name for Zhuyin 注音. Same thing!

```bash
fcitx5-configtool
```

In the config tool:
1. Click the **"+"** button
2. **Uncheck** "Only Show Current Language"
3. Search for **"Chewing"** *(this is your Zhuyin engine)*
4. Select it and click **Add**

---

## Step 10 — Test

Test in:
- **Kate** or any KDE app — switch input with `Ctrl+Space`
- **Chrome** — type in any text field

If Fcitx5 is set up correctly via Wayland, Chrome will work **without any extra flags** — unlike the old X11 setup.

---

## Troubleshooting

**Fcitx5 warning about GTK_IM_MODULE / QT_IM_MODULE:**
→ Remove those env vars from `~/.profile` (see Step 4)

**Chewing not showing in configtool:**
→ Run `sudo apt install fcitx5-chewing` then restart configtool

**Fcitx5 not starting:**
→ Make sure "Fcitx 5" is set as Virtual Keyboard in System Settings (Step 6)

**Input not working in Chrome:**
→ Confirm you're on Wayland: `echo $XDG_SESSION_TYPE` should return `wayland`

---

*Guide based on a real installation session on Kubuntu 26 — June 2026*
