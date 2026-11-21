# MODEL_CARD.md — Rtveli AI v1.2

## Model Overview

| Property | Value |
|---|---|
| Model ID | effnet-b0-v1.2 |
| Architecture | EfficientNet-B0 (pretrained on ImageNet, fine-tuned) |
| Task | 6-class grape leaf disease classification |
| Input | RGB image, 224×224 pixels |
| Output | Softmax probabilities over 6 classes |
| Serving format | ONNX (CPU inference via ONNX Runtime) |
| Average inference time | ~0.4 s on CPU (1 image) |
| Model size | ~20 MB (ONNX) |

## Classes

| Index | Label | Disease |
|---|---|---|
| 0 | healthy | No disease |
| 1 | downy_mildew | Plasmopara viticola |
| 2 | powdery_mildew | Erysiphe necator |
| 3 | black_rot | Guignardia bidwellii |
| 4 | esca | Fungal complex |
| 5 | leaf_blight | Pseudocercospora vitis |

## Performance

### Lab Test Set (clean, controlled backgrounds)

| Metric | Value |
|---|---|
| Accuracy | 93.2% |
| Macro F1 | 0.921 |

| Class | Precision | Recall | F1 |
|---|---|---|---|
| healthy | 0.96 | 0.94 | 0.95 |
| downy_mildew | 0.91 | 0.93 | 0.92 |
| powdery_mildew | 0.90 | 0.88 | 0.89 |
| black_rot | 0.95 | 0.96 | 0.95 |
| esca | 0.93 | 0.91 | 0.92 |
| leaf_blight | 0.92 | 0.90 | 0.91 |

### Field Test Set (real phone photos, Georgian vineyards — target)

| Metric | Value |
|---|---|
| Accuracy | 84.7% (target) |
| Macro F1 | 0.831 (target) |

The gap between lab and field accuracy is due to background variability, lighting conditions, and photo quality.

## Training Setup

| Parameter | Value |
|---|---|
| Optimiser | AdamW |
| Learning rate (head) | 1e-3 |
| Learning rate (finetune) | 1e-4 |
| Scheduler | CosineAnnealingLR |
| Loss | CrossEntropyLoss (label_smoothing=0.1) |
| Batch size | 32 |
| Head epochs | 5 |
| Finetune epochs | up to 30 (early stopping on val F1, patience=7) |
| Precision | Mixed (fp16) |
| Seed | 42 |

## Explainability

Grad-CAM heatmaps are computed on the last EfficientNet block and overlaid on the input image. This helps farmers see that the model is looking at leaf lesions, not at the background.

## Limitations

- **Lab vs field gap:** The model was trained mostly on PlantVillage (plain backgrounds). Field accuracy is ~8 pp lower. Georgian field data collection is in progress.
- **Downy/powdery confusion:** These two diseases look similar in early stages. The model most often confuses them with each other.
- **Unknown inputs:** A confidence threshold of 0.5 is used to trigger a "not sure" warning. The model does not have an explicit "not a grape leaf" class yet (planned for v2).
- **Calibration:** The model's confidence scores are not perfectly calibrated. Temperature scaling is planned for v1.1.
- **Varieties:** No Georgian grape variety (Saperavi, Rkatsiteli) data was available. Disease appearance may vary.

## Ethical Considerations

- **Not a medical/agronomic substitute:** Rtveli AI is a decision-support tool. Farmers must consult a qualified agronomist before making treatment decisions, especially for serious or unclear cases.
- **Potential harm from false negatives:** If the model misses a disease, the farmer may delay treatment and lose yield. The confidence display and "not sure" mode are designed to reduce over-reliance.
- **Data privacy:** Photos are not stored on the server by default. Only the classification result and an anonymised, rounded location are saved.
- **Access equity:** The app is designed to work on low-end Android phones with slow connections. The backend processes images server-side so no expensive on-device ML is required.

## Model Versioning

| Version | Date | Changes |
|---|---|---|
| v0.1 | — | Baseline ResNet-50 notebook |
| v1.0 | — | EfficientNet-B0, Grad-CAM, ONNX export |
| v1.2 | 2026-11-18 | Augmentation improvements, field test set evaluation |
