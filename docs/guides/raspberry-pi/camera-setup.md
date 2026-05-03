# Pi Camera Setup on Ubuntu 22.04 LTS

!!! info "Context"
    This guide covers the full dependency maze for the **OV5647 (Rev 1.3)** sensor on a **Raspberry Pi 4** running **Ubuntu 22.04 LTS** — including the Meson version fix, building `libcamera` from source, and all usage commands.

---

## Physical Check First

Before touching any software:

!!! warning "Hardware Checklist"
    - Ribbon cable must be in the **CSI (Camera)** port — between the HDMI and Audio jack, **NOT** the DSI (Display) port
    - Silver pins on the ribbon cable must face the **HDMI ports**
    - Check your sensor model on the green PCB — if it says **Rev 1.3**, the `dtoverlay=ov5647` line below is **mandatory**

---

## Phase 1: Firmware Configuration

The Pi 4 won't auto-detect the OV5647 Rev 1.3 sensor. You have to tell it manually.

```bash
sudo nano /boot/firmware/config.txt
```

Add these lines at the bottom:

```text
# Disable auto-detect for older Rev 1.3 sensors
#camera_auto_detect=1

# Manually load the driver for OV5647
dtoverlay=ov5647
```

Then reboot:

```bash
sudo reboot
```

!!! danger "Skip this = 'no cameras available' error every time"
    This step is mandatory regardless of what software you install.

---

## Phase 2: Install Build Dependencies

**Update the system:**

```bash
sudo apt update && sudo apt upgrade -y
```

**Install build tools and required libraries:**

```bash
sudo apt install -y git ninja-build pkg-config libepoxy-dev libjpeg-dev libtiff5-dev \
libpng-dev libboost-dev libboost-program-options-dev libdrm-dev libexif-dev \
python3-pip python3-jinja2 python3-yaml python3-ply libgnutls28-dev openssl libssl-dev
```

**Remove the old Meson:**

```bash
sudo apt remove -y meson
```

**Install the latest Meson via pip:**

```bash
sudo pip3 install meson
```

**Clear the shell command cache so it picks up the new Meson:**

```bash
hash -r
```

!!! note "Why upgrade Meson?"
    Ubuntu 22.04 ships an old Meson version that can't handle new `libcamera` build scripts. `pip3 install meson` gets the latest. `hash -r` clears the shell cache so it picks up the new one.

---

## Phase 3: Build libcamera from Source

Ubuntu's default `libcamera` package is incompatible with the Pi's camera stack. Building from source gives you proper hardware optimization.

**Clone the repository:**

```bash
cd ~
git clone https://git.libcamera.org/libcamera/libcamera.git
cd libcamera
```

**Configure for Raspberry Pi 4 (vc4 pipeline):**

```bash
meson setup build -Dpipelines=rpi/vc4 -Dipas=rpi/vc4 -Dv4l2=enabled \
-Dgstreamer=disabled -Dtest=false -Dlc-compliance=disabled -Dcam=disabled \
-Dqcam=disabled -Ddocumentation=disabled -Dprefix=/usr
```

**Compile:**

```bash
ninja -C build
```

**Install:**

```bash
sudo ninja -C build install
```

**Refresh the shared library cache:**

```bash
sudo ldconfig
```

---

## Phase 4: Build rpicam-apps from Source

These are the actual commands (`rpicam-jpeg`, `rpicam-vid`) you use to interact with the camera.

**Clone the repository:**

```bash
cd ~
git clone https://github.com/raspberrypi/rpicam-apps.git
cd rpicam-apps
```

**Configure for headless Ubuntu (no GUI dependencies):**

```bash
meson setup build -Denable_libav=disabled -Denable_drm=enabled \
-Denable_egl=disabled -Denable_qt=disabled -Denable_opencv=disabled
```

**Compile:**

```bash
ninja -C build
```

**Install:**

```bash
sudo ninja -C build install
```

**Refresh the shared library cache:**

```bash
sudo ldconfig
```

---

## Phase 5: Capturing Media

**Capture a photo:**

```bash
rpicam-jpeg -o lab_photo.jpg
```

**Record a 10-second video:**

```bash
rpicam-vid -t 10000 -o lab_video.h264
```

**Convert to MP4** (needed for browser playback — `.h264` won't play directly):

```bash
ffmpeg -framerate 30 -i lab_video.h264 -c copy lab_video.mp4
```

---

## Phase 6: Viewing Files Remotely

**Start a simple HTTP server in the folder where your files are saved:**

```bash
python3 -m http.server 8000
```

Then open your laptop's browser and go to:

```
http://<your_pi_ip>:8000
```

- Click `lab_photo.jpg` to view the photo
- Click `lab_video.mp4` to play the video

---

## Phase 7: Live Streaming

=== "Method A — Desktop Window (on Pi's GUI)"

    Run this inside the Pi's desktop terminal (via monitor or Remote Desktop):

    ```bash
    rpicam-vid -t 0 --inline -o - | ffplay -window_title "Live Feed" -i -
    ```

=== "Method B — Stream to Laptop via VLC (Low Latency)"

    **On the Pi:**

    ```bash
    rpicam-vid -t 0 --inline --listen -o tcp://0.0.0.0:8888
    ```

    **On your laptop — open VLC, press `Ctrl+N` and enter:**

    ```
    tcp/h264://your_pi_ip:8888
    ```

---

## Quick Reference

| Task | Command |
|------|---------|
| Capture photo | `rpicam-jpeg -o test.jpg` |
| Record video | `rpicam-vid -t 10000 -o test.h264` |
| Convert to MP4 | `ffmpeg -framerate 30 -i test.h264 -c copy test.mp4` |
| View files remotely | `python3 -m http.server 8000` |
| Live stream (desktop) | `rpicam-vid -t 0 --inline -o - \| ffplay -i -` |
| Live stream (VLC) | `rpicam-vid -t 0 --inline --listen -o tcp://0.0.0.0:8888` |

---

## Why This All Works

| Fix | Reason |
|-----|--------|
| `dtoverlay=ov5647` | Stops the Pi guessing and loads the exact Rev 1.3 driver |
| Meson upgrade | Old Ubuntu version couldn't parse new `libcamera` build scripts |
| Building `libcamera` from source | Ubuntu's packaged version lacks Pi-specific hardware optimizations |
| Boost libraries | C++ dependencies that let `rpicam-apps` handle your inputs correctly |