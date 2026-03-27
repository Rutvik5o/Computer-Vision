# Lab 4 — Task 1: Skin Cancer Detection with CNN
## EfficientNetB0 + Transfer Learning + Grad-CAM | HAM10000 Dataset

---

## What This Lab Does

Trains a deep CNN to classify skin lesion images into **7 diagnostic categories**
(including melanoma and other cancers) using transfer learning on the HAM10000 dataset.

Goes beyond basic accuracy to show **why** the model makes each prediction using Grad-CAM heatmaps.

---

## The Dataset: HAM10000

> *"Human Against Machine with 10,000 training images"*

- **10,015 dermoscopy images** of real skin lesions
- **7 classes**: melanoma, mole, basal cell carcinoma, actinic keratosis, benign keratosis, vascular lesion, dermatofibroma
- Source: International Skin Imaging Collaboration (ISIC)
- Heavily imbalanced (~67% are benign moles)
- Download via Kaggle API (setup instructions in notebook)

---

## What Makes This Different from a Basic CNN Lab

| Basic approach | This notebook |
|----------------|--------------|
| Custom 3–5 layer CNN | EfficientNetB0 (ImageNet pre-trained, ~5.3M params) |
| No imbalance handling | Class-weighted loss — rare cancer classes weighted 5–10× |
| Single training phase | Two-phase: frozen backbone → fine-tune last 30 layers |
| Accuracy only | Accuracy + AUC + per-class precision/recall/F1 + ROC curves |
| No explainability | Grad-CAM heatmaps show which pixels drove the decision |

---

## Architecture

```
Input Image (224×224 RGB)
        ↓
EfficientNetB0 Backbone  ← pre-trained on ImageNet, feature extraction
        ↓
GlobalAveragePooling2D   ← compress feature maps
        ↓
BatchNorm → Dense(256) → Dropout(0.4)
        ↓
Dense(128) → Dropout(0.3)
        ↓
Dense(7, softmax)        ← 7 class probabilities
```

---

## Training Strategy

**Phase 1 — 5 epochs, backbone frozen**
Only the classification head trains. Safe warm-up that avoids destroying
the pre-trained ImageNet features.

**Phase 2 — up to 20 epochs, last 30 backbone layers unfrozen**
Fine-tuning with 10× lower learning rate. The backbone gradually adapts
to dermoscopy images without forgetting general visual features.

---

## Grad-CAM: Explainability

For each prediction, Grad-CAM produces a heatmap showing which skin region
influenced the decision most.

- **Hot (red/yellow)** = model focused here
- **Cool (blue)** = model ignored this region

This is critical for clinical trust — we want the model to look at the
*lesion* itself, not skin tone, ruler artifacts, or hair.

---

## Notebook Structure (38 cells)

| Cells | Content |
|-------|---------|
| 1–2 | Install, imports, GPU setup |
| 3–4 | Download HAM10000 via Kaggle |
| 5–7 | EDA: class distribution, sample images |
| 8–9 | Preprocessing: stratified split, class weights |
| 10 | Augmentation demo |
| 11 | Build EfficientNetB0 model |
| 12 | Phase 1 training |
| 13 | Phase 2 fine-tuning |
| 14 | Training history plots |
| 15–16 | Evaluate: accuracy, AUC, classification report |
| 17–18 | Confusion matrix (counts + normalised) |
| 19–21 | Grad-CAM implementation + visualisation |
| 22–23 | Per-class precision/recall/F1 bar chart |
| 24–25 | ROC curves (one per class) |
| 26 | Summary dashboard + conclusion |

---

## How to Run

1. Open in **Google Colab**
2. Set runtime: `Runtime → Change runtime type → T4 GPU`
3. Get Kaggle API key: kaggle.com → Account → Create New API Token → download `kaggle.json`
4. Run all cells — when prompted, upload `kaggle.json`

**Expected runtime:** ~30–45 minutes (dataset download + training)

---

## Output Files

| File | Description |
|------|-------------|
| `class_distribution.png` | Bar + pie chart showing class imbalance |
| `sample_lesions.png` | 3 examples of each lesion type |
| `augmentation_demo.png` | Same image with 8 augmented variations |
| `training_history.png` | Loss/accuracy/AUC/recall across epochs |
| `confusion_matrix.png` | Counts + normalised heatmaps |
| `gradcam_results.png` | Grad-CAM overlays for all 7 classes |
| `per_class_metrics.png` | Precision/Recall/F1 bar chart |
| `roc_curves.png` | ROC curve per class with AUC |
| `best_model_final.keras` | Saved trained model weights |

---

## Key Concepts Explained in Notebook

**Transfer Learning** — Using a model trained on one task (ImageNet classification)
as a starting point for a different but related task (skin cancer classification).
Works because both tasks involve recognising visual patterns.

**Class Weighting** — During loss computation, errors on rare classes are penalised more.
A missed melanoma gets a 5× bigger penalty than a missed mole.

**Grad-CAM** — Computes the gradient of the predicted class score with respect to
the last convolutional layer's feature maps. High gradient = that feature map
mattered for this prediction. The result is projected back onto the image as a heatmap.

**AUC (Area Under ROC Curve)** — Better than accuracy for imbalanced data.
AUC = 0.5 means random guessing; AUC = 1.0 means perfect.
