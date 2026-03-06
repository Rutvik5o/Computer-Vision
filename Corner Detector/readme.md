# 📐 Harris & FAST Corner Detection

> A computer vision notebook that finds "corners" in images using two classic algorithms — Harris and FAST — and compares them side by side.

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

Imagine you're looking at a photo. A **corner** is a point in the image where edges meet sharply — like the corner of a building, a table edge, or a window frame. These corner points are super useful in CV tasks like tracking objects, stitching photos, and 3D mapping.

This project uses **two popular algorithms** to automatically find these corner points:

| Algorithm | What it does (simply) |
|---|---|
| **Harris** | Scans the image mathematically to find spots where brightness changes sharply in multiple directions. Marks them with **red dots**. |
| **FAST** | Looks at a circle of 16 pixels around each point. If enough of them are brighter or darker than the center, it's a corner. Marks them with **green dots**. |

Both algorithms run on the same input image, and the results are shown side-by-side for easy comparison.

---

## 🔬 What's Actually Implemented

### 1. Harris Corner Detection (Custom, from scratch)
- Computes image gradients using **Sobel filters**
- Applies **Gaussian smoothing** to reduce noise
- Calculates the **Harris response score (R)** for every pixel
- Uses **Non-Maximum Suppression (NMS)** — keeps only the strongest local corners
- Applies a **percentile threshold** to filter weak detections
- Refines corner positions to **sub-pixel accuracy** using least-squares fitting
- Runs at **multiple scales** (Gaussian pyramid) to detect corners of different sizes

### 2. FAST Corner Detection (Custom, from scratch)
- Tests each pixel against a **circle of 16 surrounding pixels**
- A point is a corner if **12+ consecutive pixels** are significantly brighter or darker
- Assigns a **score** to each corner based on total pixel difference
- Uses **Non-Maximum Suppression** to remove duplicate nearby corners

### 3. Visual Comparison
- Displays original image with **Harris corners (red)** and **FAST corners (green)** side by side using Matplotlib

---

## Pipeline

```
Input Image (e.g., amazon.jpg)
        |
        v
  Convert to Grayscale
        |
     +--+--+
     |     |
     v     v
  HARRIS  FAST
  -----------------------------------------------
  Sobel gradients    Circle of 16 pixels test
  Gaussian smooth    12+ consecutive bright/dark
  Harris R score     Score = sum of differences
  NMS + Threshold    NMS (keep best per region)
  Sub-pixel refine
  Multi-scale
     |     |
     +--+--+
        v
  Draw dots on image
  (Red = Harris, Green = FAST)
        |
        v
  Side-by-side plot
```

---

## Libraries Used

| Library | Purpose |
|---|---|
| `opencv-python` | Image I/O, pyramid downscaling, drawing |
| `numpy` | Array math and matrix operations |
| `matplotlib` | Displaying results |
| `scipy.ndimage` | Sobel gradients, Gaussian/uniform filters, max filter |

---

## How to Run

### In Google Colab

1. Open the notebook in Google Colab
2. Run all cells
3. Place your image (e.g., `amazon.jpg`) in the Colab working directory
4. Call:
```python
run_pipeline_on_image("amazon.jpg")
```
5. Two plots appear side-by-side — Harris corners (red) and FAST corners (green)

### Requirements

```bash
pip install opencv-python numpy matplotlib scipy
```

---

## File Structure

```
Corner Detector/
|
+-- Corner_Detector.ipynb   # Main notebook
+-- README.md               # This file
```

---

## Key Functions

| Function | What it does |
|---|---|
| `harris_response()` | Computes Harris corner score R for every pixel |
| `harris_nms()` | Keeps only local maximum corners |
| `harris_threshold()` | Removes weak corners below a percentile |
| `harris_detect_points()` | Combines NMS + threshold to get final corners |
| `harris_refine_subpixel()` | Improves corner location accuracy to sub-pixel level |
| `gaussian_pyramid()` | Builds multi-scale image pyramid for multi-scale Harris |
| `fast_score()` | Calculates strength score for a FAST corner |
| `fast_corners_with_scores()` | Full FAST detection with scores |
| `run_pipeline_on_image()` | Runs both detectors and shows comparison plot |

---

## Limitations

- Both algorithms are **classical** (non-deep learning) — less robust than modern detectors like SIFT, ORB, or SuperPoint
- Harris can be **slow** on large images due to per-pixel computation
- FAST can produce **many false positives** in textured or noisy regions
- Neither handles large **rotation** or **scale changes** well without multi-scale support

---

## References

- Harris & Stephens (1988) — A Combined Corner and Edge Detector
- Rosten & Drummond (2006) — Machine Learning for High Speed Corner Detection (FAST)
- OpenCV Corner Detection Docs: https://docs.opencv.org/4.x/dc/d0d/tutorial_py_features_harris.html

---

## License

Educational project for a Computer Vision course.
