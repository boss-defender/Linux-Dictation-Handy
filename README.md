# LInux-Dictation-Handy

# Handy Voice Dictation on Fedora KDE Wayland

A clean setup guide for **Handy + KWtype + Parakeet V3** on Fedora KDE Wayland.

This setup is designed for a **Fedora KDE Plasma Wayland** desktop where Handy needs to inject the transcribed text into the currently focused application.

> **Important:** On KDE Wayland, use **KWtype**, not Wtype. Wtype relies on the Wayland virtual keyboard protocol that KWin does not provide in the required way. KWtype uses KDE's KWin Fake Input protocol instead.

---

## 1. Install all KWtype dependencies

Copy and paste this **single command**:

```bash
sudo dnf install -y git gcc-c++ meson ninja-build pkgconf-pkg-config qt6-qtbase-devel kwayland-devel libxkbcommon-devel wayland-devel gtk-layer-shell
```

---

## 2. Download, build, and install KWtype

### Download

```bash
cd ~/Downloads
git clone https://github.com/Sporif/KWtype.git
```

### Enter the folder

```bash
cd ~/Downloads/KWtype
```

### Configure the build

```bash
meson setup --buildtype=release --prefix=/usr/local build
```

### Build

```bash
meson compile -C build
```

### Install system-wide

```bash
sudo meson install -C build
```

---

## 3. Verify KWtype

Check whether Fedora can find KWtype:

```bash
command -v kwtype
```

Expected:

```text
/usr/local/bin/kwtype
```

Check that KWtype runs:

```bash
kwtype --help
```

### If `command -v kwtype` returns nothing

First check whether KWtype exists:

```bash
ls -l /usr/local/bin/kwtype
```

If the file exists, add `/usr/local/bin` to your shell's `PATH`.

**Bash:**

```bash
grep -qxF 'export PATH="/usr/local/bin:$PATH"' ~/.bashrc || echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**Zsh / Oh My Zsh:**

```bash
grep -qxF 'export PATH="/usr/local/bin:$PATH"' ~/.zshrc || echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Fish:**

```fish
fish_add_path /usr/local/bin
```

Then verify again:

```bash
command -v kwtype
```

Expected:

```text
/usr/local/bin/kwtype
```

### Test KWtype

Open Kate, Firefox, Chrome, or another application with a text field.

Click inside the text field, then run:

```bash
kwtype "Hello from KWtype on Fedora KDE Wayland"
```

The text should appear in the focused text field.

> **Do not continue until this test works.**

---

# 4. Install Handy from the official GitHub release

Open the official Handy release page:

https://github.com/cjpais/Handy/releases

Download the latest:

```text
Handy-x.x.x-x.x86_64.rpm
```

For an AMD/Intel 64-bit PC, use:

```text
x86_64.rpm
```

Do **not** download the ARM/aarch64 RPM.

Put the downloaded RPM in:

```text
~/Downloads
```

---

## 5. Install Handy with DNF

Open a terminal:

```bash
cd ~/Downloads
```

Then install the downloaded RPM:

```bash
sudo dnf install ./Handy-*.x86_64.rpm
```

> **Do not use `rpm -i`.**
>
> Use DNF so Fedora can resolve the required dependencies automatically.

---

# 6. Verify Handy

Check the executable:

```bash
command -v handy
```

Check the version:

```bash
handy --version
```

Launch Handy:

```bash
handy
```

---

# 7. Configure Handy

Open:

```text
Handy → Settings
```

Use these settings:

| Setting                | Recommended value                                       |
| ---------------------- | ------------------------------------------------------- |
| **Typing Tool**        | `kwtype`                                                |
| **Paste Method**       | `Direct`                                                |
| **Auto Submit**        | `Off`                                                   |
| **Clipboard Handling** | `Don't Modify Clipboard` **or** `Copy to Clipboard`     |
| **Model Unload**       | `Never` **or** `After 15 minutes` **or** `After 1 hour` |
| **Overlay Position**   | `Bottom`                                                |
| **Overlay**            | `Live`                                                  |
| **Start Hidden**       | `On`                                                    |
| **Launch on Startup**  | `On`                                                    |
| **Show Tray Icon**     | `On`                                                    |

### Most important settings

Make sure these are exactly:

```text
Typing Tool:  kwtype
Paste Method: Direct
Auto Submit:  Off
```

Do **not** select:

```text
wtype
```

for KDE Wayland.

---

# 8. Install the Parakeet V3 transcription model

In Handy:

```text
Settings → Models
```

Select:

```text
Parakeet V3
```

Download/install the model from inside Handy.

### Why Parakeet V3?

Parakeet V3 is a particularly good choice for a CPU-based Linux system.

It provides:

* Fast local transcription
* Good accuracy
* CPU-oriented performance
* Automatic language detection
* Completely offline transcription

For a system such as a Ryzen 7 7700 with 16 GB RAM and no dedicated GPU, Parakeet V3 is a strong default choice.

---

# 9. Select your microphone

In Handy's audio/microphone settings:

```text
Microphone → Select your microphone
```

Make sure the correct input device is selected.

Then test recording.

---

# 10. Test Handy + Parakeet + KWtype

Open any text field.

For example:

```text
Firefox
Chrome
Kate
Discord
Telegram
ChatGPT
```

Start Handy's transcription shortcut.

Speak normally.

Stop the recording.

The complete pipeline should be:

```text
Microphone
   ↓
Handy
   ↓
Parakeet V3
   ↓
Direct text input
   ↓
KWtype
   ↓
KWin / KDE Wayland
   ↓
Focused application
```

The transcribed text should automatically appear in the focused text field.

---

# 11. Optional KDE global shortcut

Handy supports:

```bash
handy --toggle-transcription
```

You can assign this command to any KDE global shortcut.

Go to:

```text
System Settings
→ Shortcuts
→ Custom Shortcuts
→ New
→ Global Shortcut
→ Command/URL
```

Use:

```text
Name:
Toggle Handy Transcription
```

Command:

```bash
handy --toggle-transcription
```

Example shortcut:

```text
Super + O
```

---

# 12. Final configuration

Your final Handy configuration should look like this:

```text
Transcription Model:  Parakeet V3

Typing Tool:          kwtype
Paste Method:         Direct
Auto Submit:          Off

Clipboard Handling:   Don't Modify Clipboard
                      OR
                      Copy to Clipboard

Model Unload:         Never
                      OR
                      After 15 minutes
                      OR
                      After 1 hour

Overlay Position:     Bottom
Overlay:              Live

Start Hidden:         On
Launch on Startup:    On
Show Tray Icon:       On
```

---

# 13. Final verification

Run:

```bash
command -v kwtype
```

Expected:

```text
/usr/local/bin/kwtype
```

Then:

```bash
kwtype --help
```

Then:

```bash
command -v handy
```

Then:

```bash
handy --version
```

Everything is ready when:

```text
KWtype works by itself
        ↓
Handy detects KWtype
        ↓
Handy uses Direct + KWtype
        ↓
Parakeet V3 transcribes
        ↓
Text appears in the focused application
```

---

# Troubleshooting

## Handy does not show `kwtype`

Run:

```bash
command -v kwtype
```

If it returns nothing:

```bash
fish_add_path /usr/local/bin
```

Then restart the terminal and try:

```bash
command -v kwtype
```

If KWtype was installed into `~/.local/bin`:

```bash
fish_add_path ~/.local/bin
```

Then restart the terminal.

---

## Handy uses Wtype

Open:

```text
Handy → Settings → Typing Tool
```

Set:

```text
kwtype
```

Do not use:

```text
wtype
```

on KDE Wayland.

---

## KWtype works in terminal but Handy cannot see it

Completely close Handy and start it again.

Then open:

```text
Settings → Typing Tool
```

`kwtype` should now be available.

---

## Handy transcribes but does not type anything

First test:

```bash
kwtype "Test text"
```

while a text field is focused.

If this works, check Handy:

```text
Typing Tool:  kwtype
Paste Method: Direct
```

Also make sure the target application still has focus when transcription finishes.

Keeping the Handy overlay configured correctly is important because an overlay can sometimes take focus away from the application receiving the text.

---

# Official Projects

## Handy

https://github.com/cjpais/Handy

## KWtype

https://github.com/Sporif/KWtype

## Handy Releases

https://github.com/cjpais/Handy/releases

---

# Quick Install Summary

```text
1. Install Fedora dependencies
2. Build KWtype
3. Install KWtype to /usr/local
4. Verify `kwtype`
5. Test `kwtype` in a text field
6. Download latest Handy x86_64 RPM
7. Install with DNF
8. Open Handy
9. Select Parakeet V3
10. Set Direct + KWtype
11. Turn Auto Submit Off
12. Configure overlay/startup/tray settings
13. Test voice typing
```

**Fedora KDE Wayland + Handy = KWtype + Direct**

**Do not use Wtype for KDE Wayland.**
