# Lab 3 — Padding & Stride in CNNs

A hands-on experiment showing how two small CNN settings — **padding** and **stride** — affect model size, speed, and accuracy on the MNIST handwritten digit dataset.

---

## What This Lab Is About

When a convolutional layer scans an image, you can control two things:

| Setting | What it does |
|---------|-------------|
| **Padding** | Whether to add zeros around the image edges before scanning |
| **Stride** | How many pixels the filter jumps per step |

These two settings seem minor, but they have real effects on how much the model learns, how many parameters it has, and how fast it runs.

---

## The 5 Experiments

| Config | Padding | Stride | Expected Effect |
|--------|---------|--------|----------------|
| Same padding, stride 1 | same | 1 | Full detail, largest model |
| Valid padding, stride 1 | valid | 1 | Slightly smaller output |
| Same padding, stride 2 | same | 2 | Faster, fewer params |
| Valid padding, stride 2 | valid | 2 | Even smaller maps |
| Same padding, stride 3 | same | 3 | Fastest, but least detail |

---

## Key Concepts (Plain English)

**Padding = "same"**  
Zeros are added around the image so the output stays the same size as the input. Edges get equal attention as the center.

**Padding = "valid"**  
No zeros added. The filter only scans where it fully fits, so the output shrinks. Corner pixels get scanned less.

**Stride = 1**  
The filter moves one pixel at a time — slow, thorough, captures fine detail.

**Stride = 2 or 3**  
The filter jumps multiple pixels — faster, uses less memory, but might miss subtle patterns.

---

## Results Summary

From the experiments (actual numbers from a GPU run):

| Config | Parameters | Train Time | Test Accuracy |
|--------|------------|------------|--------------|
| Same padding, stride 1 | 421,642 | 19.6s | **99.07%** |
| Valid padding, stride 1 | 225,034 | 16.8s | **99.08%** |
| Same padding, stride 2 | 53,002 | 13.8s | 98.36% |
| Valid padding, stride 2 | 28,426 | 14.0s | 97.05% |
| Same padding, stride 3 | 28,426 | 14.9s | 96.13% |

**Takeaways:**
- Stride 1 models are more accurate but heavier
- Valid padding + stride 1 gives the best accuracy-to-size ratio here
- Higher strides dramatically reduce parameters and train time, at the cost of accuracy
- For simple datasets like MNIST, even the lightest model still gets ~96%

---

## Notebook Structure

```
Cell 1   — Imports + setup
Cell 2   — Load & preprocess MNIST
Cell 3   — Build CNN function (configurable padding/stride)
Cell 4   — Define experiments
Cell 5   — Run all 5 experiments, collect results
Cell 6   — Display comparison table
Cell 7   — Visualize feature maps (what the network sees)
Cell 8   — Bonus: Binary classification (is it a 7?)
Cell 9   — Bonus: Full 10-digit classification with prediction demo
```

---

## How to Run

1. Open in **Google Colab** (GPU recommended — set via `Runtime → Change runtime type → T4 GPU`)
2. Run all cells top to bottom
3. The experiment loop will take a few minutes — each model trains for 5 epochs

To try a different digit in the prediction demo, change:
```python
digit_to_test = 3  # ← change this to 0–9
```

---

## Requirements

- Python 3.x
- TensorFlow 2.x (`pip install tensorflow`)
- NumPy, Matplotlib, Pandas (usually pre-installed in Colab)

---

## Files

| File | Description |
|------|-------------|
| `Lab_3_Padding_Stride_commented.ipynb` | Main notebook with inline comments |
| `README.md` | This file |
