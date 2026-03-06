# 🚦 Traffic Monitoring & Violation Detection System

> A comprehensive video analytics system using YOLOv8 and multi-object tracking to detect traffic violations and monitor road conditions in real time.

---

## 📋 Project Info

| Field | Details |
|---|---|
| **Author** | Rutvik Prajapati |
| **Course** | Computer Vision |
| **Instructor** | Mr. Chintan Patel |
| **Platform** | Google Colab |

---

## 🧐 What Does This Project Do? (Simple Explanation)

This project watches **highway and road videos** and automatically detects traffic violations. It can:
- Catch vehicles running a **red light**
- Measure **vehicle speeds** on a highway
- Flag cars going the **wrong way**
- Read **license plates** at toll booths

It uses **YOLOv8** (a state-of-the-art object detector) to find vehicles in every frame, then tracks each vehicle across frames to perform analysis.

---

## 🔹 Modules Overview

### 1. Red-Light Violation Detection
- Detects vehicles crossing the **stop line** during a red signal
- Tracks vehicle IDs to avoid duplicate violation logging
- Visual overlays: stop line, bounding boxes, "VIOLATION!" alert, violation counter

### 2. Highway Speed & Congestion Monitoring
- Measures vehicle speed between **enter** and **exit** lines using real-world distances and frame timestamps
- Detects **stopped vehicles** based on minimal displacement over multiple frames
- Computes **rolling average speed** to indicate traffic congestion
- Visual overlays: enter/exit lines, speed per vehicle, "STOPPED" label, average speed display

### 3. Wrong-Way Driving Detection
- Tracks the **trajectory** (path history) of each vehicle
- Computes overall motion vector to flag vehicles going against the allowed direction
- Visual overlays: red bounding box, "WRONG WAY!" alert, live counter

### 4. Toll Booth / ANPR (Automatic Number Plate Recognition)
- Detects vehicles approaching a **toll booth ROI** (region of interest)
- Crops and zooms the vehicle for better OCR accuracy
- Extracts license plate text using **Tesseract OCR**
- Visual overlays: toll ROI rectangle, vehicle boxes, detected plate text

---

## 🔄 System Pipeline

```
Input Video (Highway / Road)
        |
        v
Frame-by-Frame Extraction
        |
        v
YOLOv8 Object Detection (detect vehicles)
        |
        v
Multi-Object Tracking (ByteTrack / BoT-SORT)
        |
        v
Per-Module Analysis:
  +-- Red Light: line crossing check
  +-- Speed: entry/exit timestamp delta
  +-- Wrong Way: trajectory vector analysis
  +-- ANPR: ROI crop + Tesseract OCR
        |
        v
Draw Annotations on Frame
        |
        v
Save Output Video / Display Live
```

---

## 🛠️ Technologies Used

| Library / Tool | Purpose |
|---|---|
| `ultralytics` (YOLOv8) | Vehicle detection in each frame |
| `ByteTrack` / `BoT-SORT` | Multi-object tracking across frames |
| `OpenCV` | Video I/O, frame annotation, drawing |
| `Tesseract OCR` | License plate text extraction |
| `NumPy` | Coordinate math, trajectory analysis |

---

## 📁 File Structure

```
Traffic monitoring Video analytics/
|
+-- CVP_Traffic_Monitoring_Videos.ipynb   # Main notebook
+-- Highway video.mp4                     # Input highway footage
+-- highway_output.mp4                    # Output: speed monitoring
+-- highway_traffic_analysis.mp4          # Output: traffic analysis
+-- red_light_output.mp4                  # Output: red-light violations
+-- readme.md                             # Project readme
```

---

## 📊 Output Videos

| Output File | What it shows |
|---|---|
| `red_light_output.mp4` | Vehicles caught running red lights |
| `highway_output.mp4` | Speed and congestion monitoring |
| `wrong_way_output.mp4` | Wrong-direction vehicles flagged |
| `toll_output.mp4` | License plates read at toll booth |

---

## 🚀 How to Run

1. Open `CVP_Traffic_Monitoring_Videos.ipynb` in Google Colab
2. Install dependencies:
```bash
pip install ultralytics pytesseract opencv-python
apt-get install tesseract-ocr
```
3. Upload your video or use the included `Highway video.mp4`
4. Run the desired module cell
5. Download the annotated output video

---

## ⚙️ Key Technical Concepts

- **Line crossing detection** — using vehicle trajectory points vs. defined lines
- **Speed estimation** — real-world distance ÷ time between two virtual lines
- **Trajectory-based wrong-way detection** — motion vector from position history
- **ROI zooming + OCR** — crop vehicle bounding box, upscale, run Tesseract
- **Rolling average** — congestion score from last N vehicle speeds

---

## ⚠️ Limitations

- Speed accuracy depends on correct calibration of real-world pixel-to-meter ratio
- Tesseract OCR can struggle with low-resolution or angled plates
- YOLO may miss heavily occluded vehicles in dense traffic
- Wrong-way detection may produce false positives at intersections

---

## 📖 References

- [Ultralytics YOLOv8 Docs](https://docs.ultralytics.com/)
- [ByteTrack Paper](https://arxiv.org/abs/2110.06864)
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [OpenCV Video I/O](https://docs.opencv.org/4.x/dd/d43/tutorial_py_video_display.html)

---

## 📄 License

Educational project for a Computer Vision course.
