# Lab 9 & 10 — Image Stitching and Panorama Creation

| | |
|---|---|
| **Name** | Rutvik Prajapati |
| **Course** | Computer Vision |
| **Instructor** | Mr. Chintan Patel |
| **Platform** | Google Colab |
| **Notebook** | `Image_Stitchnig___Panorama_Creation.ipynb` |

---

## Overview

This lab implements a complete image stitching pipeline from scratch using classical Computer Vision techniques. Multiple overlapping photographs are joined into a single wide panoramic image — the same process your phone uses in panorama mode, but built step by step so every component is visible and understandable.

The notebook also compares the manual implementation against OpenCV's built-in `cv2.Stitcher` class and benchmarks two feature detectors (ORB vs SIFT) on the same image pair.

---

## Objective

- Understand each stage of the panorama stitching pipeline
- Implement feature detection, matching, homography estimation, warping, and blending manually
- Compare hard-cut blending vs alpha blending for seam removal
- Use OpenCV's `Stitcher` in both PANORAMA and SCANS modes
- Compare ORB and SIFT feature detectors in terms of keypoint count and match quality

---

## Dataset

**Kaggle:** [`gaurvivishnoi/panorama`](https://www.kaggle.com/datasets/gaurvivishnoi/panorama)

Downloaded directly in Colab using `kagglehub` — no manual file upload or Kaggle CLI needed. Credentials are written programmatically in the notebook.

```python
import kagglehub
path = kagglehub.dataset_download('gaurvivishnoi/panorama')
```

---

## Dependencies

```bash
pip install kagglehub opencv-contrib-python --upgrade
```

| Library | Purpose |
|---|---|
| `opencv-contrib-python` | Core CV — ORB, SIFT, Stitcher, homography, warping |
| `numpy` | Matrix operations, coordinate transforms |
| `matplotlib` | Visualising images and matches |
| `kagglehub` | Dataset download without Kaggle CLI |

> **Note:** `opencv-contrib-python` is required (not just `opencv-python`) to access SIFT and the Stitcher class.

---

## Pipeline

### What is Image Stitching?

When you rotate a camera slightly between shots, adjacent photos overlap. Stitching finds those overlapping regions and merges all photos into one wide panorama by computing the geometric transformation between each pair.

```
Input Images → Feature Detection → Feature Matching → Homography → Warping → Blending → Panorama
```

---

## Notebook Structure

### Part 0 — Install Libraries
Installs `kagglehub` and `opencv-contrib-python`.

---

### Part 1 — Configure Kaggle & Download Dataset
Writes Kaggle API credentials directly in code (no upload required), then downloads the `gaurvivishnoi/panorama` dataset. Prints the folder tree of downloaded files.

---

### Part 2 — Load & Visualise Input Images
Scans all image files recursively, groups them by parent folder, and picks the folder with the most images as the panorama sequence. Images are resized to max width 900px to keep Colab RAM usage manageable.

---

### Part 3 — Manual Stitching Pipeline

#### Step 3A — Feature Detection & Matching (ORB)

**ORB** (Oriented FAST + Rotated BRIEF) detects keypoints in both images. Matching uses `BFMatcher` with Hamming distance, filtered by **Lowe's ratio test** (threshold 0.75) to remove ambiguous matches.

```
Raw matches → Lowe ratio test (0.75) → Good matches (~30-40% kept)
```

The top 50 matches are visualised with connecting lines between both images.

#### Step 3B — Homography Estimation with RANSAC

A 3×3 homography matrix `H` is computed from the good matches using `cv2.findHomography` with RANSAC (reprojection threshold = 5.0px). RANSAC automatically discards outlier matches that don't fit the geometric model.

```
Good matches → RANSAC → Homography H (3×3) + inlier mask
```

#### Step 3C — Warp & Stitch Two Images

Image 2 is warped into Image 1's coordinate frame using `cv2.warpPerspective`. A translation matrix `T` shifts coordinates so nothing falls outside the canvas. Image 1 is then overlaid on top — this produces a **hard-cut** result where the seam is visible.

#### Step 3D — Sequential Stitch (Full Panorama)

All images are merged one by one: `pano = img[0]`, then `img[1]`, `img[2]`, etc. are added sequentially. After stitching, **black borders are cropped** using the same threshold + boundingRect approach as the reference code.

---

### Part 4 — OpenCV Built-in Stitcher

`cv2.Stitcher_create()` is tested in two modes:

| Mode | Best For |
|---|---|
| `PANORAMA` | Rotating camera shots (typical phone panoramas) |
| `SCANS` | Flat document or aerial scans (mostly translation) |

`setPanoConfidenceThresh(0.5)` is applied to improve success rate on moderate-overlap images. Status codes are mapped to human-readable messages for easy debugging.

---

### Part 5 — Comparison: Manual vs OpenCV Stitcher

Side-by-side display of all successful results with output dimensions printed for each method.

---

### Part 6 — Alpha Blending to Fix the Seam

The hard-cut from Part 3 has a sharp visible seam. Alpha blending fixes this by weighting each pixel proportionally based on coverage:

- Where only Image 1 covers: uses Image 1 fully
- Where only Image 2 covers: uses Image 2 fully  
- In the overlap zone: linearly blends between both images

```
alpha = mask1 / (mask1 + mask2)
result = img1 * alpha + img2 * (1 - alpha)
```

Hard-cut vs alpha-blend are displayed side by side for direct comparison.

---

### Part 7 — Feature Detector Comparison: ORB vs SIFT

Both detectors are run on the same image pair. Results are printed in a comparison table and keypoints are visualised with scale-indicating circles.

| | ORB | SIFT |
|---|---|---|
| Speed | Fast | Slower |
| Descriptor | Binary (Hamming) | Float (L2) |
| Patent | Free | Free since 2020 |
| Typical matches | Fewer | More, better distributed |

---

### Part 8 — Save All Results

All outputs are saved to `/content/outputs/`:

| File | Description |
|---|---|
| `manual_panorama.jpg` | Sequential manual stitch (all images, cropped) |
| `alpha_blend_stitch.jpg` | Alpha-blended result for first two images |
| `opencv_panorama_mode.jpg` | OpenCV Stitcher — PANORAMA mode |
| `opencv_scans_mode.jpg` | OpenCV Stitcher — SCANS mode |

Download from the **Files panel** (folder icon on the left sidebar in Colab).

---

## Key Concepts Summary

| Step | What We Did | Key Takeaway |
|---|---|---|
| Dataset | `gaurvivishnoi/panorama` via `kagglehub` | No manual download needed |
| Feature Detection | ORB keypoints | Fast, patent-free, works well for outdoor scenes |
| Matching | BFMatcher + Lowe ratio test (0.75) | Removes ~60–70% of wrong matches |
| Homography | RANSAC, 5px reprojection threshold | Handles remaining outliers robustly |
| Warping | `perspectiveTransform` + `warpPerspective` | Projects pixels into a shared coordinate frame |
| Blending | Hard-cut → Alpha blend | Alpha blend significantly reduces visible seam |
| OpenCV Stitcher | PANORAMA + SCANS modes | Better quality — exposure correction built in |
| ORB vs SIFT | Keypoint + match count | SIFT usually more matches; ORB is faster |

---

## Common Failure Cases

- **Too little overlap** (< 20%) between adjacent photos — not enough shared keypoints
- **Textureless regions** (plain sky, blank walls) — no detectable corners
- **Large parallax** — objects at very different depths moving between shots
- **Big exposure differences** — matching fails across very different lighting

**Fix:** lower `setPanoConfidenceThresh(0.3)` or capture images with more overlap.

---

## How to Run

1. Open the notebook in **Google Colab**
2. Run **Cell 1** — installs libraries
3. Run **Cell 2** — writes Kaggle credentials (automatic, no upload)
4. Run **Cell 3** — downloads the dataset
5. Run all remaining cells in order — each part builds on the previous

> All cells are designed to run top-to-bottom without modification.
