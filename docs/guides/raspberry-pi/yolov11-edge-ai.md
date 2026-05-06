# YOLOv11 Edge AI on Raspberry Pi

!!! info "What this guide covers"
    A complete pipeline from a blank Raspberry Pi to a high-speed, edge-optimized **YOLOv11 object tracker** — with dedicated branches for both the Pi Camera (ribbon cable) and USB Webcam.

---

## Phase 1: Clean Room Environment

Always build inside an isolated virtual environment so Ubuntu doesn't accidentally pull in broken server-grade NVIDIA packages.

**Create and enter the project folder:**

```bash
mkdir -p ~/yolo && cd ~/yolo
```

**Create the virtual environment:**

```bash
python3 -m venv venv
```

**Activate it:**

```bash
source venv/bin/activate
```

**Install Pi-Safe (CPU Only) PyTorch:**

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
```

**Install YOLO and NCNN:**

```bash
pip install ultralytics ncnn
```

---

## Phase 2: Hardware Crash Prevention

!!! danger "Do this before running anything"
    By default, the math library **OpenBLAS** will panic and throw an `Illegal instruction (core dumped)` error because it doesn't recognize the Pi's CPU architecture. This fix is permanent and survives reboots.

**Apply the permanent fix:**

```bash
echo 'export OPENBLAS_CORETYPE=ARMV8' >> ~/.bashrc
```

**Reload your shell so it takes effect immediately:**

```bash
source ~/.bashrc
```

---

## Phase 3: Export to NCNN Format

The standard PyTorch `.pt` file is too heavy for the Pi. You must convert it to **NCNN** — an ultra-fast, C++ optimized format built for edge devices.

!!! note "Make sure your venv is active before running this"

**Run the export:**

```bash
python3 -c "from ultralytics import YOLO; YOLO('yolo11n.pt').export(format='ncnn')"
```

Wait 1–3 minutes. When complete, you will see a `yolo11n_ncnn_model/` folder generated in your directory.

---

## Phase 4: Camera Scripts

Both scripts use two key optimizations:

| Optimization | Reason |
|-------------|--------|
| `imgsz=320` | Halves the input resolution → roughly **doubles FPS** on the Pi |
| `FOURCC=YUYV` | Forces standard color format → prevents OpenCV `reshape` crashes |

=== "📷 Pi Camera (Ribbon Cable)"

    **Create the script:**

    ```bash
    nano ~/yolo/pi_cam_yolo.py
    ```

    **Paste this code:**

    ```python
    import cv2
    from ultralytics import YOLO
    import time

    print("[INFO] Loading NCNN Edge model...")
    model = YOLO("yolo11n_ncnn_model", task="detect")

    print("[INFO] Opening Pi Camera (requires libcamerify)...")
    cap = cv2.VideoCapture(0, cv2.CAP_V4L2)

    # Force YUYV to prevent OpenCV reshape crashes
    cap.set(cv2.CAP_PROP_FOURCC, cv2.VideoWriter_fourcc(*'YUYV'))
    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)

    if not cap.isOpened():
        print("[ERROR] Cannot open Pi Camera. Did you use libcamerify?")
        exit(1)

    fps = 0
    while True:
        ret, frame = cap.read()
        if not ret or frame is None:
            time.sleep(0.1)
            continue

        # imgsz=320 doubles your FPS!
        t0 = time.time()
        
        results = model(frame, verbose=False)[0]
        # results = model(frame, imgsz=320, verbose=False)[0]
        
        inf_ms = (time.time() - t0) * 1000
        fps = fps * 0.8 + 0.2 * (1000.0 / max(inf_ms, 1.0))

        annotated = results.plot()
        cv2.putText(annotated, f"Pi Cam FPS: {fps:.1f}", (10, 30),
                    cv2.FONT_HERSHEY_SIMPLEX, 1.0, (0, 255, 0), 2)
        cv2.imshow("Pi Camera YOLO Stream", annotated)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()
    ```

=== "🔌 USB Webcam"

    **Create the script:**

    ```bash
    nano ~/yolo/usb_webcam_yolo.py
    ```

    **Paste this code:**

    ```python
    import cv2
    from ultralytics import YOLO
    import time

    print("[INFO] Loading NCNN Edge model...")
    model = YOLO("yolo11n_ncnn_model", task="detect")

    print("[INFO] Opening USB Webcam natively...")
    # Change index to 0 if the USB cam is the ONLY camera plugged in
    cap = cv2.VideoCapture(1, cv2.CAP_V4L2)

    cap.set(cv2.CAP_PROP_FOURCC, cv2.VideoWriter_fourcc(*'YUYV'))
    cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 480)

    if not cap.isOpened():
        print("[ERROR] Cannot open USB Camera. Try changing the index to 0 or 2.")
        exit(1)

    fps = 0
    while True:
        ret, frame = cap.read()
        if not ret or frame is None:
            time.sleep(0.1)
            continue

        t0 = time.time()
        
        results = model(frame, verbose=False)[0]
        # results = model(frame, imgsz=320, verbose=False)[0]

        inf_ms = (time.time() - t0) * 1000
        fps = fps * 0.8 + 0.2 * (1000.0 / max(inf_ms, 1.0))

        annotated = results.plot()
        cv2.putText(annotated, f"USB Cam FPS: {fps:.1f}", (10, 30),
                    cv2.FONT_HERSHEY_SIMPLEX, 1.0, (255, 0, 0), 2)
        cv2.imshow("USB Webcam YOLO Stream", annotated)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()
    ```

---

## Phase 5: Launch Commands

!!! warning "Activate your venv first"
    Always run `source venv/bin/activate` before launching any script.

=== "📷 Pi Camera"

    You **must** use the `libcamerify` wrapper so OpenCV can understand the ribbon cable:

    ```bash
    libcamerify python3 pi_cam_yolo.py
    ```

=== "🔌 USB Webcam"

    Do **not** use the wrapper. USB is natively understood by Linux:

    ```bash
    python3 usb_webcam_yolo.py
    ```

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| `"Camera index out of range"` or `"Not a video capture device"` | A previous script crashed and locked the hardware. Run `sudo killall python3` |
| `"Illegal instruction (core dumped)"` | The terminal forgot your CPU architecture. Run `export OPENBLAS_CORETYPE=ARMV8` |
| `"Bad new number of rows in function 'reshape'"` | Camera is sending raw sensor data. Ensure `cap.set(cv2.CAP_PROP_FOURCC, cv2.VideoWriter_fourcc(*'YUYV'))` is in your script |
| Can't find the USB Webcam index | Run `v4l2-ctl --list-devices` to see exactly which `/dev/videoX` it was assigned |