# 👁️ Face and Eye Detection using Classical Computer Vision

> A real-time face and eye detection system built with OpenCV's Haar Cascade Classifier — part of a Computer Vision course project.

---

## 📋 Project Overview

This project implements a **classical computer vision pipeline** for detecting faces and eyes in images. Unlike deep learning-based approaches, it relies on traditional hand-crafted features using the **Haar Cascade Classifier** — a well-established algorithm for object detection.

| Field       | Details                    |
|-------------|----------------------------|
| **Author**  | Rutvik Prajapati           |
| **Course**  | Computer Vision            |
| **Instructor** | Mr. Chintan Patel       |
| **Platform**| Google Colab               |

---

## 🎯 Objective

Implement a face and eye detection system that:
- Loads an input image (uploaded by the user)
- Converts it to grayscale for processing
- Detects faces using Haar Cascade
- Detects eyes within each detected face region
- Draws bounding boxes and visualizes the result

---

## 🧠 Theory

### Classical Computer Vision
Classical CV uses **hand-crafted features** and traditional algorithms rather than neural networks. It is interpretable, lightweight, and effective for well-defined tasks like face detection.

### Haar Cascade Classifier
The Haar Cascade algorithm works by:
- Extracting **Haar-like features** from image regions
- Using **integral images** for fast computation
- Applying **AdaBoost** for selecting the most discriminative features
- Using a **cascade of classifiers** for efficient, staged detection

**Common applications:**
- Face detection
- Eye detection
- Basic object detection

### Detection Pipeline

```
Input Image
    │
    ▼
Convert to Grayscale
    │
    ▼
Detect Faces (Haar Cascade)
    │
    ▼
For each Face Region → Detect Eyes
    │
    ▼
Draw Bounding Boxes (Green = Face, Blue = Eyes)
    │
    ▼
Display Result
```

---

## 🛠️ Technologies Used

| Library       | Purpose                          |
|---------------|----------------------------------|
| `opencv-python` | Image processing & detection   |
| `numpy`       | Array/matrix operations          |
| `matplotlib`  | Visualizing detection results    |
| `google.colab`| File upload in Colab environment |

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install opencv-python numpy matplotlib
```

### Running in Google Colab

1. Open the notebook in [Google Colab](https://colab.research.google.com/)
2. Run all cells sequentially
3. When prompted, **upload an image** (e.g., `pic.jpg`) containing faces
4. The notebook will automatically download the Haar Cascade XML files and process the image

---

## 📂 File Structure

```
Classical CV Pipeline/
│
├── ClassicalCVPipeline.ipynb   # Main Jupyter notebook
└── README.md                   # Project documentation
```

---

## ⚙️ How It Works

### Step 1 — Install & Import Libraries
```python
pip install opencv-python
import cv2, numpy as np, matplotlib.pyplot as plt
```

### Step 2 — Upload Image (Colab)
```python
from google.colab import files
uploaded = files.upload()
```

### Step 3 — Load Image
```python
img = cv2.imread("pic.jpg")
```

### Step 4 — Load Haar Cascades
```python
# Downloads pre-trained XML classifiers from OpenCV GitHub
face_cascade = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
eye_cascade  = cv2.CascadeClassifier("haarcascade_eye.xml")
```

### Step 5 — Detect Faces & Eyes
```python
gray  = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
faces = face_cascade.detectMultiScale(gray, 1.3, 5)

for (x, y, w, h) in faces:
    cv2.rectangle(img, (x, y), (x+w, y+h), (0, 255, 0), 2)  # Green box = face
    roi_gray = gray[y:y+h, x:x+w]
    eyes = eye_cascade.detectMultiScale(roi_gray, 1.3, 5)
    for (ex, ey, ew, eh) in eyes:
        cv2.rectangle(roi, (ex, ey), (ex+ew, ey+eh), (255, 0, 0), 2)  # Blue box = eye
```

### Step 6 — Display Result
```python
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.axis("off")
plt.title("Face and Eye Detection Result")
plt.show()
```

---

## 📊 Sample Output

```
✅ Image loaded successfully
✅ Haar Cascades Loaded
Faces detected: 4
Eyes detected: 0
```

> **Note:** Eye detection may return 0 in some cases depending on image quality, lighting, or face angle — this is a known limitation of Haar Cascade classifiers.

---

## ⚠️ Limitations

- **Haar Cascades** are sensitive to lighting conditions, face orientation, and occlusion
- Eye detection may fail if eyes are partially closed or the face is at an angle
- Does not perform well on non-frontal faces
- Less accurate compared to deep learning-based detectors (e.g., MTCNN, RetinaFace)

---

## 📖 References

- [OpenCV Haar Cascades Documentation](https://docs.opencv.org/4.x/db/d28/tutorial_cascade_classifier.html)
- [Viola-Jones Object Detection Framework (original paper)](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf)
- [OpenCV GitHub — Pre-trained Cascades](https://github.com/opencv/opencv/tree/master/data/haarcascades)

---

## 📄 License

This project is for educational purposes as part of a Computer Vision course assignment.
