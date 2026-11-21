# DATA.md — Rtveli AI Data Card

## Dataset Summary

| Property | Value |
|---|---|
| Task | Multi-class image classification (grape leaf diseases) |
| Classes | 6 (healthy, downy_mildew, powdery_mildew, black_rot, esca, leaf_blight) |
| Total images (target) | ~6,000–8,000 |
| Split | 70% train / 15% val / 15% test, stratified by class |
| Image resolution | Variable → resized to 224×224 for training |

## Data Sources

### 1. PlantVillage Dataset

- **URL:** https://plantvillage.psu.edu
- **Licence:** CC BY 4.0
- **Classes used:** Grape – Black rot (1180 images), Grape – Esca/Black measles (1383 images), Grape – Leaf blight (1076 images), Grape – Healthy (423 images)
- **Known bias:** All photos taken on plain backgrounds in controlled lab conditions. No field variability.

### 2. Kaggle Grape Leaf Disease Dataset

- **URL:** https://www.kaggle.com/datasets/rm1000/grape-disease-dataset-original
- **Licence:** CC BY-SA 4.0
- **Classes used:** Downy mildew (~600 images), Powdery mildew (~600 images)
- **Known bias:** Mixed controlled and field photos. Some label noise reported.

### 3. Research Publication Datasets

- Papers: doi:10.3390/agriculture10100464, doi:10.1016/j.compag.2019.104948
- Used as supplementary data for rare disease classes
- Licence: Research use / contact authors

### 4. Georgian Vineyard Photos (Planned)

- **Status:** To be collected in Kakheti and Imereti during growing season
- **Method:** Consenting farmers photograph leaves with guidance on framing and lighting
- **Licence:** Own data, CC0 (no personal data collected)

## Class Distribution (approximate, training set)

| Class | Count | % |
|---|---|---|
| healthy | ~300 | 11% |
| downy_mildew | ~700 | 25% |
| powdery_mildew | ~700 | 25% |
| black_rot | ~500 | 18% |
| esca | ~350 | 13% |
| leaf_blight | ~250 | 9% |

Class imbalance is handled with a weighted random sampler (WeightedRandomSampler) during training.

## Preprocessing

1. Resize to 256×256, then centre-crop to 224×224 (validation/test)
2. Random resized crop to 224×224 (training)
3. Normalise with ImageNet mean/std: mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]

## Augmentations (training only)

| Augmentation | Parameter |
|---|---|
| Random horizontal flip | p=0.5 |
| Random vertical flip | p=0.2 |
| Rotation | ±30° |
| Color jitter | brightness/contrast/saturation ±30% |
| Gaussian blur | kernel 3–7, p=0.3 |
| Coarse dropout | 8 holes, 20×20 px, p=0.3 |

## Known Limitations and Biases

- PlantVillage photos have plain white/grey backgrounds. Models trained only on these will fail on field photos with soil, sky, or other vegetation in the background.
- Class "healthy" is significantly underrepresented in PlantVillage. Weighted sampling mitigates this.
- Downy and powdery mildew can look similar in early stages; the model may confuse these two classes.
- Geographic bias: most source data is from North America and Europe. Georgian vineyard conditions (varieties, climate, soil) may not be represented.
- No data from Georgian varieties (Saperavi, Rkatsiteli, Mtsvane) specifically.
