# Traffic Monitoring & Violation Detection System

A comprehensive traffic monitoring solution using **YOLOv8** and multi-object tracking. Designed to handle multiple real-world scenarios including red-light violations, highway speed monitoring, wrong-way driving, and toll booth ANPR.

---

## 🔹 Modules Overview

### 1️⃣ Red-Light Violation Detection
- 🚦 Detects vehicles crossing the **stop line** during a red signal.
- 🆔 Tracks vehicle IDs and maintains history to avoid duplicate violation logging.
- 🖌️ Visual overlays:
  - Stop line
  - Vehicle bounding boxes and IDs
  - "VIOLATION!" alert
  - Violation counter on the frame

---

### 2️⃣ Highway Speed & Congestion Monitoring
- 🛣️ Measures vehicle speed between **enter** and **exit lines**.
- ⏱️ Calculates speed using real-world distance and frame timestamps.
- 🛑 Detects stopped vehicles based on minimal displacement over multiple frames.
- 📊 Computes rolling average speed to indicate **traffic congestion**.
- 🖌️ Visual overlays:
  - Enter/Exit lines
  - Speeds per vehicle
  - "STOPPED" label for stationary vehicles
  - Average speed display

---

### 3️⃣ Wrong-Way Driving Detection
- ↩️ Detects vehicles moving **against the allowed direction**.
- Tracks recent positions of each vehicle (trajectory history).
- Computes overall motion vector to flag wrong-way driving.
- 🖌️ Visual overlays:
  - Red bounding box for wrong-way vehicles
  - "WRONG WAY!" alert
  - Live counter of wrong-way vehicles

---

### 4️⃣ Toll Booth / ANPR (Automatic Number Plate Recognition)
- 🚗 Detects vehicles approaching a toll booth ROI.
- 🔍 Crops and zooms the vehicle bounding box for better OCR.
- 📝 Extracts license plate text using **Tesseract OCR**.
- 🖌️ Visual overlays:
  - Toll ROI rectangle
  - Vehicle bounding boxes
  - Last detected license plate on frame center
  - Plate text near vehicle

---

## ⚙️ Common Features
- 🖥️ Supports **YOLOv8 models** (`yolov8n.pt`, `yolov8s.pt`) and **trackers** (ByteTrack / BoT-SORT).
- 🔄 Modular pipeline using `process_frame_fn` for custom per-frame processing.
- 💾 Option to **display live video** and/or **save annotated output**.
- 🖌️ Consistent visual indicators across modules:
  - Bounding boxes, IDs
  - Alerts and violation labels
  - Metrics like speed, congestion, plate numbers

---

## 📌 Key Technical Concepts
- Line crossing detection using trajectory points.
- Vehicle speed estimation using real-world distances.
- Trajectory-based detection for wrong-way driving.
- ROI-based zooming and OCR for toll booth ANPR.
- Rolling metrics for congestion analysis.

---

## 🗂️ Output
- Red-light violations → `/content/red_light_output.mp4`
- Highway speed & congestion → `/content/highway_output.mp4`
- Wrong-way driving → `/content/wrong_way_output.mp4`
- Toll / ANPR → `/content/toll_output.mp4`

---

> This modular framework can be extended for:
> - Automatic traffic signal recognition
> - Advanced license plate detection models
> - Real-time dashboard monitoring
