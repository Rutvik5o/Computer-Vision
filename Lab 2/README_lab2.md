# 🔬 Lab 2 — Image Filtering & Enhancement

> Second lab of the Computer Vision course covering image filtering, noise removal, edge detection, and histogram operations.

---

## 📋 Project Info

| Field | Details |
|---|---|
| **Author** | Rutvik Prajapati |
| **Course** | Computer Vision |
| **Instructor** | Mr. Chintan Patel |
| **Platform** | Google Colab |

---

## 🧐 What Does This Lab Do? (Simple Explanation)

This lab teaches you how to **clean up, sharpen, and analyse images** mathematically. You'll learn how blurring removes noise, how edge detection finds boundaries in an image, and how histograms tell you about the brightness distribution of a photo.

Think of it as giving an image a "filter" — like Instagram, but with the actual math behind it.

---

## 🎯 Topics Covered

- **Smoothing / Blurring filters:**
  - Average (Box) Filter
  - Gaussian Blur
  - Median Blur (great for salt-and-pepper noise)
  - Bilateral Filter (smooths while preserving edges)

- **Sharpening:**
  - Laplacian filter
  - Unsharp masking

- **Edge Detection:**
  - Sobel operator (horizontal & vertical gradients)
  - Canny Edge Detector (multi-stage, robust)

- **Morphological Operations:**
  - Erosion, Dilation
  - Opening, Closing

- **Histogram Analysis:**
  - Plotting grayscale and color histograms
  - Histogram Equalization (improves contrast)
  - CLAHE (Contrast Limited Adaptive Histogram Equalization)

- **Thresholding:**
  - Simple binary threshold
  - Otsu's automatic thresholding

---

## 🔄 Lab Pipeline

```
Load Image
    |
    v
Add Noise (optional, for testing)
    |
    v
Apply Filters (Gaussian, Median, Bilateral)
    |
    v
Edge Detection (Sobel / Canny)
    |
    v
Morphological Ops (Erode / Dilate)
    |
    v
Histogram Analysis & Equalization
    |
    v
Thresholding
    |
    v
Display & Compare Results
```

---

## 🛠️ Libraries Used

| Library | Purpose |
|---|---|
| `opencv-python` | Filters, edge detection, morphology |
| `numpy` | Pixel math and array operations |
| `matplotlib` | Plotting images and histograms |

---

## 🚀 How to Run

1. Open `Lab2.ipynb` in Google Colab
2. Run all cells top to bottom
3. Upload or use a sample image when prompted

### Requirements

```bash
pip install opencv-python numpy matplotlib
```

---

## 📂 File Structure

```
Lab 2/
|
+-- Lab2.ipynb    # Main lab notebook
+-- README.md     # This file
```

---

## ⚠️ Notes

- **Gaussian Blur** works best for general noise; **Median Blur** is better for impulse (salt-and-pepper) noise
- **Canny** is the most commonly used edge detector in practice
- **Histogram Equalization** improves contrast but can over-amplify noise — use CLAHE for better results

---

## 📖 References

- [OpenCV Filtering Tutorial](https://docs.opencv.org/4.x/d4/d13/tutorial_py_filtering.html)
- [OpenCV Canny Edge Detection](https://docs.opencv.org/4.x/da/d22/tutorial_py_canny.html)
- [OpenCV Histograms](https://docs.opencv.org/4.x/d1/db7/tutorial_py_histogram_begins.html)

---

## 📄 License

Educational lab for a Computer Vision course.
