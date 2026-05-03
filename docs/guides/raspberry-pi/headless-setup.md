# Headless Raspberry Pi Deployment Guide

!!! tip "Why this guide exists"
    The standard Ubuntu Desktop is not "remotely friendly" by default. The Server version is a clean slate you can control. Headless troubleshooting is a huge part of working with servers and IoT as a CSE student.

---

## Phase 1: The Imager (Pre-Boot)

!!! danger "Do NOT choose Ubuntu Desktop"
    Choose **Ubuntu Server 22.04 LTS (64-bit)** when flashing. The Desktop version will cause headless issues later.

1. Click **Next** and select **Edit Settings**.

2. **General Tab** — fill in:
    - Hostname (e.g., `sparkplug`)
    - Username and password
    - Your exact Wi-Fi SSID and password

3. **Services Tab** — check **Enable SSH** and select **Use password authentication**.

4. Click **Write** to flash the image.

---

## Phase 2: Initial Access & Updates

Once the Pi boots, find its IP address using a network scanner, then:

**1. SSH into the Pi:**

```bash
ssh username@your_ip_address
```

**2. Update the system — this is CRITICAL:**

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Phase 3: The "Stable Desktop" Stack

The default Ubuntu GNOME desktop crashes on headless setups because it needs a physical monitor to stay stable. The solution is **XFCE4** — lightweight, fast, and monitor-independent.

Run these commands in order:

**1. Install the desktop environment and the remote server:**

```bash
sudo apt install xfce4 xfce4-goodies xrdp -y
```

!!! note
    If a purple screen appears during install, press `Tab` to highlight **OK** and hit `Enter`.

**2. Add your user to the security group:**

```bash
sudo adduser your_username ssl-cert
```

**3. Configure the desktop identity** — fixes the "auto-closing window" bug:

```bash
echo "xfce4-session" > ~/.xsession
```

**4. The "Headless Permission" fix:**

```bash
sudo sh -c 'echo "allowed_users=anybody" > /etc/X11/Xwrapper.config'
```

**5. The "Double-Login / Crash" fix:**

Open the startup script:

```bash
sudo nano /etc/xrdp/startwm.sh
```

Scroll to the bottom and paste these two lines **above** the last `test` and `exec` lines:

```bash
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```

Save and exit: `Ctrl+O` → `Enter` → `Ctrl+X`

**6. Restart the XRDP service:**

```bash
sudo systemctl restart xrdp
```

---

## Phase 4: Connecting from Windows

1. Open **Remote Desktop Connection** on Windows.
2. Enter the Pi's IP address.
3. Log in with your username and password.

---

## Why This Works

| Component | Reason |
|-----------|--------|
| **Ubuntu Server** | Lets you inject Wi-Fi and SSH settings before the Pi even turns on |
| **XFCE4** | GNOME crashes without a physical monitor — XFCE doesn't care |
| **`.xsession` file** | Acts as a signpost, telling Remote Desktop which desktop environment to launch on login |
| **`startwm.sh` edit** | Clears conflicting session variables that cause crashes or double-login loops |
| **`Xwrapper.config`** | Grants permission for any user to start an X display without a physical screen |

---

!!! success "You're ready!"
    You can now use your laptop screen, keyboard, and mouse to build Django backends, React frontends, or Machine Learning models directly on the Pi — no monitor needed.