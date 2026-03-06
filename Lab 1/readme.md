# 🔬 Lab 1 — Introduction to Image Processing with OpenCV

> First lab of the Computer Vision course introducing fundamental image processing operations using OpenCV and NumPy.

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

This lab is your **first hands-on introduction** to working with images in Python. You learn how a computer "sees" an image — as a grid of numbers — and how to perform basic operations like loading, displaying, resizing, color conversion, and pixel manipulation using **OpenCV**.

Think of it as learning to handle and manipulate a photo before doing anything smart with it.

---

## 🎯 Topics Covered

- Loading and displaying images using OpenCV and Matplotlib
- Understanding image representation (NumPy arrays, pixel values)
- Color space conversions (BGR → RGB, BGR → Grayscale, BGR → HSV)
- Basic image operations:
  - Resizing and scaling
  - Cropping regions of interest (ROI)
  - Flipping and rotating
- Drawing shapes on images (lines, rectangles, circles, text)
- Accessing and modifying individual pixel values
- Saving processed images to disk

---

## 🔄 Lab Pipeline

```
Load Image (cv2.imread)
      |
      v
Display Original (matplotlib)
      |
      v
Color Conversions (Grayscale, HSV, RGB)
      |
      v
Pixel-level Operations (crop, flip, resize)
      |
      v
Draw Annotations (shapes, text)
      |
      v
Save Output (cv2.imwrite)
```

---

## 🛠️ Libraries Used

| Library | Purpose |
|---|---|
| `opencv-python` | Core image I/O and processing |
| `numpy` | Image as array — pixel math |
| `matplotlib` | Visualizing images inline in Colab |

---

## 🚀 How to Run

1. Open `Lab1.ipynb` in Google Colab
2. Run all cells top to bottom
3. Upload or use a sample image when prompted

### Requirements

```bash
pip install opencv-python numpy matplotlib
```

---

## 📂 File Structure

```
Lab 1/
|
+-- Lab1.ipynb    # Main lab notebook
+-- README.md     # This file
```

---

## ⚠️ Notes

- OpenCV loads images in **BGR** format, not RGB — always convert before displaying with Matplotlib
- Images are stored as **NumPy arrays** — shape is `(height, width, channels)`

---

## 📖 References

- [OpenCV Python Docs](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)
- [NumPy for Images](https://numpy.org/doc/stable/)

---

## 📄 License

Educational lab for a Computer Vision course.
