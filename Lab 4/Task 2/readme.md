# Lab 4 - Task 2: Face Recognition with FaceNet & Triplet Loss
## Olivetti Faces | InceptionResnetV1 | Triplet Loss | t-SNE

---

## Why Olivetti Instead of LFW?

LFW from sklearn was causing **black images** due to a data format issue:

```
LFW problem:
  lfw.images dtype  = float32
  lfw.images range  = [0.0, 255.0]   <- NOT [0.0, 1.0]
  matplotlib imshow expects float in [0.0, 1.0]
  -> Values like 156.3 get clipped to 1.0
  -> Entire image renders as white or black
```

**Olivetti Faces** solves this completely:
- `olivetti.images` is float64 in **[0.0, 1.0]** — exactly what matplotlib expects
- Downloads in 1 second from sklearn (no Kaggle, no external URLs)
- 400 clean images, 40 people, 10 per person — perfect for learning
- Balanced: every person has exactly the same number of photos

---

## Image Display Rules (Why No Black Images)

Every image goes through a strict two-pipeline system:

```python
# STORAGE: always uint8 [0-255]
images = np.array([to_rgb_uint8(img) for img in images_raw])

# DISPLAY pipeline: uint8 -> float [0,1] for matplotlib
def display_img(uint8_img):
    return uint8_img.astype(np.float32) / 255.0

axes[i].imshow(display_img(images[idx]), cmap='gray')  # always correct

# MODEL pipeline: uint8 -> normalised tensor [-1,1] for FaceNet
model_transform = T.Compose([
    T.ToPILImage(),
    T.Resize((160, 160)),
    T.ToTensor(),
    T.Normalize(mean=[0.5,0.5,0.5], std=[0.5,0.5,0.5])
])
```

The two pipelines are completely separate. Model tensors [-1,1] are never displayed.
Display arrays [0,1] are never fed to the model.

---

## Install Fix (Prevents numpy.dtype Error)

```bash
# WRONG - uninstalls Colab's torch, breaks numpy:
pip install facenet-pytorch

# CORRECT - keeps Colab's torch intact:
pip install facenet-pytorch --no-deps
```

Workflow: Cell 1 -> Restart session -> Run from Cell 2.

---

## Dataset

**Olivetti Faces (AT&T Database of Faces)**
- 400 images, 40 people, 10 photos each
- 64x64 pixels, grayscale
- Varying: lighting, expressions, glasses, head position
- Built into scikit-learn, no download required

---

## Architecture

```
Face Image (uint8 RGB)
    -> model_transform -> float tensor [-1,1], 160x160
    -> InceptionResnetV1 (VGGFace2 pre-trained)
    -> L2 Normalise
    -> 512-D unit embedding vector

Distance(embed_A, embed_B) < threshold -> same person
```

---

## Notebook Structure (43 cells)

| Cell | Content |
|------|---------|
| 1 | Safe install (--no-deps) + restart instructions |
| 2 | Imports + GPU check |
| 3 | Load Olivetti, convert to uint8 RGB |
| 4-5 | Visualise samples + within-person variation |
| 6 | Load FaceNet + define display_img / img_to_tensor |
| 7-8 | Extract 512-D embeddings for all 400 images |
| 9 | 80/20 person-level train/val split |
| 10-11 | TripletDataset + DataLoaders |
| 12 | TripletLoss implementation |
| 13-14 | Fine-tune setup + training loop (10 epochs) |
| 15 | Loss curve |
| 16-17 | Re-extract embeddings + separation ratio |
| 18-19 | Build verification pairs + calibrate threshold |
| 20-21 | Distance distribution + ROC + accuracy plots |
| 22-23 | t-SNE 2D embedding visualisation |
| 24-25 | Face identification demo |
| 26-27 | Visual query vs gallery matches |
| 28-29 | Positive/negative pair visualisation |
| 30 | Summary + conclusion |

---

## How to Run

1. Open in Google Colab
2. Runtime -> Change runtime type -> T4 GPU
3. Run Cell 1 only
4. Runtime -> Restart session
5. Run all cells from Cell 2 onwards

Expected runtime: ~10-15 minutes (Olivetti is small)

---

## Output Files

| File | Description |
|------|-------------|
| `olivetti_samples.png` | 10 photos per person, first 6 people |
| `within_person_variation.png` | All 10 photos of one person |
| `triplet_loss_curve.png` | Train/val loss per epoch |
| `verification_performance.png` | Distance dist + ROC + accuracy curve |
| `tsne_embeddings.png` | 2D t-SNE of all 40 people |
| `face_identification.png` | Query vs top-3 gallery matches |
| `verification_pairs.png` | Same/different person pairs with distances |
