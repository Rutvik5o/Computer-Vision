# 📄 Xerox JBIG2 — Image Compression & The Xerox Scanning Bug

> A computer vision notebook investigating the infamous Xerox JBIG2 compression bug — where scanned documents had digits silently substituted — and implementing safe vs. lossy compression comparisons.

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

This project studies a **real-world scanning disaster**: Xerox photocopiers using JBIG2 compression would sometimes **swap digits in scanned documents** — a "6" could become a "8", a "1" could become a "7" — without anyone noticing. This is dangerous for medical records, financial documents, and legal papers.

The notebook:
1. **Explains the Xerox JBIG2 bug** — what it is, why it happened
2. **Demonstrates lossy vs. lossless compression** — showing when compression is safe and when it's dangerous
3. **Simulates the digit-substitution problem** using image processing techniques
4. **Compares compression outputs** visually to highlight data integrity risks

---

## 🎯 What's Covered

### The Xerox JBIG2 Bug
- JBIG2 is a **lossy compression algorithm** for binary (black & white) images
- Xerox WorkCentre scanners used it to reduce file sizes
- The algorithm clusters similar-looking character patches and replaces them with a single representative — causing **digit substitution errors**
- Example: the digit **"6"** in one part of a document silently replaced with **"8"** from another

### Lossless vs. Lossy Compression
- **Lossless (safe):** PNG — every pixel is preserved exactly; no data lost
- **Lossy (dangerous for documents):** JPEG / JBIG2 — approximates pixel data to save space, can introduce errors

### Image Processing Demonstrations
- Loading and binarizing document images
- Patch-based similarity comparison (how JBIG2 matches characters)
- Visual side-by-side comparison of original vs. compressed outputs
- Highlighting regions where compression introduces errors

---

## 🔄 Pipeline

```
Input Document Image
        |
        v
Binarize (Threshold to Black & White)
        |
        v
Simulate JBIG2 Patch Clustering
  (group visually similar character regions)
        |
        v
Replace patches with cluster representative
  (this is where digit substitution happens)
        |
        v
Compare Original vs. Compressed Output
        |
        v
Highlight changed regions
        |
        v
Demonstrate Lossless (PNG) as safe alternative
```

---

## 🛠️ Libraries Used

| Library | Purpose |
|---|---|
| `opencv-python` | Image loading, binarization, display |
| `numpy` | Pixel array operations, patch comparison |
| `matplotlib` | Side-by-side visual comparisons |
| `Pillow (PIL)` | Image saving in PNG / JPEG formats |

---

## 📁 File Structure

```
Xerox JBIG2/
|
+-- Xerox.ipynb                              # Main exploration notebook
+-- Xerox_JBIG2.ipynb                        # Full JBIG2 analysis notebook
+-- images.png                               # Sample document image
+-- img.png                                  # Test image 1
+-- img2.png                                 # Test image 2
+-- text.png                                 # Text document for compression test
+-- lossless_safe.png                        # Lossless compression output (safe)
+-- lossy_danger.jpg                         # Lossy compression output (risky)
+-- your_binary_image.png                    # Binarized test image
+-- your_grayscale_image.png                 # Grayscale test image
+-- 214374691-181c9f14-...png                # Reference/comparison image
```

---

## 🚀 How to Run

1. Open `Xerox_JBIG2.ipynb` in Google Colab
2. Run all cells sequentially
3. Observe the visual comparisons between lossless and lossy outputs

### Requirements

```bash
pip install opencv-python numpy matplotlib Pillow
```

---

## ⚠️ Key Takeaway

> **Never use lossy compression (JPEG, JBIG2) for critical documents** — medical records, legal contracts, financial statements. Always use **lossless formats (PNG, TIFF)** where accuracy of every pixel matters.

The Xerox bug affected millions of documents worldwide before it was discovered in 2013 by David Kriesel.

---

## 📖 References

- [David Kriesel's Xerox Bug Discovery (2013)](http://www.dkriesel.com/en/blog/2013/0802_xerox-workcentres_are_switching_written_numbers_when_scanning)
- [JBIG2 Wikipedia](https://en.wikipedia.org/wiki/JBIG2)
- [Lossless vs Lossy Compression — OpenCV](https://docs.opencv.org/4.x/)

---

## 📄 License

Educational project for a Computer Vision course.
