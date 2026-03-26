# Lab 3 — Task 2: Object Detection with YOLOv8
### Backbone Complexity × Input Resolution — A Systematic Comparison

---

## What This Lab Does

This notebook benchmarks **YOLOv8** (3 model sizes) across **3 input resolutions** on a
diverse set of real-world images — giving a complete picture of the speed vs accuracy
trade-off in modern single-stage object detection.

**Total experiments: 9 configs** (3 models × 3 resolutions)

---

## Why YOLOv8 Instead of Faster R-CNN?

YOLOv8 (2023) is the current state-of-the-art single-stage detector. Comparison:

| | Faster R-CNN | YOLOv8 |
|--|--|--|
| Architecture | Two-stage (RPN + detector) | Single-stage end-to-end |
| Speed | ~5–10 FPS | 30–160 FPS |
| Small objects | Stronger | Good at higher resolutions |
| Setup complexity | High (custom anchors, RPN tuning) | One `pip install` |
| Pretrained weights | COCO | COCO |

---

## Experiment Design

### Variables

**Backbone sizes tested:**

| Model | Parameters | Notes |
|-------|------------|-------|
| `yolov8n` | ~3.2M | Nano — fastest, smallest |
| `yolov8s` | ~11M | Small — balanced |
| `yolov8m` | ~25M | Medium — most accurate |

**Input resolutions tested:** `320` · `640` · `1280` pixels

All models use **pre-trained COCO weights** — no custom training needed.

### Test Images

6 diverse scenes downloaded from Unsplash (free license):
street, kitchen, sports, animals, traffic, outdoor market

### Metrics Collected

- Average inference latency (ms per image)
- Average detections per image
- Average confidence score
- Class diversity heatmap (80 COCO classes)

---

## Results Overview

*(Exact numbers depend on hardware — re-run to see yours)*

General trends:
- Higher resolution → more detections, better confidence, slower speed
- YOLOv8m finds ~30% more objects than YOLOv8n on the same scene
- 320→1280px increases latency ~4–6× but detections ~1.5–2×
- **YOLOv8s @ 640px** is the practical sweet-spot

---

## Notebook Structure

```
Cell 1   Setup — install ultralytics + torchinfo
Cell 2   Imports & GPU check
Cell 3   Download 6 diverse test images
Cell 4   Image preview grid
Cell 5   Define 9 experiment configs (3 models × 3 resolutions)
Cell 6   Parameter count comparison
Cell 7   Run all 9 experiments
Cell 8   Results table (color-highlighted best values)
Cell 9   Line plots: Latency / Detections / Confidence vs Resolution
Cell 10  Visual grid: same image, 3 backbones @ 640px
Cell 11  Visual grid: same model, 3 resolutions (YOLOv8s)
Cell 12  Class detection heatmap
Cell 13  Speed vs Confidence scatter (bubble chart)
Cell 14  Auto-computed key findings printout
Cell 15  Conclusion & recommendations table
```

---

## How to Run

1. Open in **Google Colab**
2. Set runtime: `Runtime → Change runtime type → T4 GPU`
3. Run all cells: `Runtime → Run all`

**Expected runtime:** ~5–10 minutes on T4 GPU

---

## Requirements

Everything installs in Cell 1:

```bash
pip install ultralytics torchinfo
```

YOLOv8 weights (~50 MB each) download automatically on first use from Ultralytics servers.

---

## Sample Outputs Generated

| File | Description |
|------|-------------|
| `benchmark_plots.png` | 3-panel line chart (latency / detections / confidence) |
| `visual_comparison_backbones.png` | Same image through 3 model sizes |
| `visual_comparison_resolution.png` | Same model at 3 resolutions |
| `class_heatmap.png` | COCO class frequency per model |
| `speed_vs_accuracy.png` | Bubble scatter: speed vs confidence |

---

## Key Concepts

**Single-stage detection**  
YOLO predicts boxes and classes in one forward pass — no separate region proposal step.
This is why it's 10–30× faster than Faster R-CNN.

**Backbone size**  
The feature extractor. Bigger backbone → richer spatial features → finds more objects,
especially small or partially occluded ones.

**Input resolution (imgsz)**  
Image is resized to a square before inference. Higher resolution gives more pixel detail
to work with but costs more memory and compute time.

**Confidence threshold**  
Only detections ≥ 0.30 confidence are kept. Lowering finds more boxes but adds false positives.

**COCO**  
80-class benchmark dataset (person, car, dog, chair, bottle, etc.) used for pre-training.
