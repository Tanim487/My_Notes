# USB Webcam Setup on Ubuntu 22.04 LTS

!!! info "Context"
    Unlike the Pi Camera, USB webcams require **no firmware edits, no custom compiles, and no driver installation**. They use the Linux kernel's built-in **Universal Video Class (UVC)** drivers — plug in and go.

!!! success "Pi Camera + USB Camera = No Conflict"
    Because they use completely different pathways in the OS, you can run both cameras **at the exact same time** without any conflicts. The Pi Camera uses the internal ISP and `libcamera` stack, while the USB camera bypasses all of that entirely.

---

## Phase 1: Verify the Camera is Recognized

**Check the USB bus — confirm your camera appears:**

```bash
lsusb
```

**Find the video device node:**

```bash
ls -l /dev/video*
```

!!! note "Which `/dev/video` node is yours?"
    If the Pi Camera is active, it usually claims `/dev/video0`.
    Your USB camera will likely register as `/dev/video1` or `/dev/video2`.
    Take note of the node number — you'll need it in every command below.

---

## Phase 2: Install Required Tools

USB cameras process their own images internally, so you need standard Linux video tools — **not** `rpicam-apps`.

**Update the system:**

```bash
sudo apt update
```

**Install the tools:**

```bash
sudo apt install -y fswebcam ffmpeg v4l-utils
```

| Tool | Purpose |
|------|---------|
| `fswebcam` | Captures still photos from a USB camera |
| `ffmpeg` | Records video from a USB camera |
| `v4l-utils` | Video4Linux utilities — inspect and manage video devices |

---

## Phase 3: Capturing Media

!!! warning "Check your device node first"
    The commands below use `/dev/video1` as an example. Replace it with whatever node your camera registered as in Phase 1.

**Capture a photo:**

```bash
fswebcam -d /dev/video1 -r 1280x720 --no-banner usb_photo.jpg
```

**Record a video:**

```bash
ffmpeg -f v4l2 -framerate 30 -video_size 1280x720 -i /dev/video1 output_usb.mp4
```

---

## Phase 4: Viewing Files Remotely

**Start an HTTP server in the folder where your files are saved:**

```bash
python3 -m http.server 8000
```

Then open your laptop's browser and go to:

```
http://<your_pi_ip>:8000
```

- Click `usb_photo.jpg` to view the photo
- Click `output_usb.mp4` to play the video

---

## Why This Works So Easily

| Reason | Explanation |
|--------|-------------|
| **Built-in UVC Drivers** | The Linux kernel natively understands the processors inside USB cameras — no manual driver setup needed |
| **Hardware Independence** | The USB pathway completely bypasses the Pi's internal ISP and the custom `libcamera` stack you built for the Pi Camera |
| **Simultaneous Operation** | Pi Camera and USB Camera use different OS pathways — they can run at the same time with zero conflicts |