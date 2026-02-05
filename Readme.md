# Klasifikasi Kematangan Buah Kakao

Binary Image Classification menggunakan Deep Learning (CNN) dengan pendekatan Transfer Learning

---

## Deskripsi Proyek

Proyek ini merupakan bagian dari **Capstone Project kategori AI Engineer** yang bertujuan untuk membangun model **klasifikasi kematangan buah kakao berbasis citra digital**.
Model melakukan klasifikasi ke dalam **dua kelas utama**:

* **Matang**
* **Mentah** (gabungan kelas *Immature* dan *Overmature*)

Model dikembangkan menggunakan **TensorFlow & Keras** dengan pendekatan **Transfer Learning berbasis MobileNetV2**, serta dirancang menggunakan **pipeline yang terstruktur dan profesional**, sehingga siap untuk pengembangan lanjutan maupun tahap deployment (mobile / edge device).

---

## Dataset

Dataset yang digunakan merupakan dataset publik dari Kaggle:

**Nama Dataset**: Cocoa Ripeness Dataset TCS 01
**Sumber Dataset**: Kaggle – Cocoa Ripeness Dataset TCS 01 https://www.kaggle.com/datasets/juanfelipeheredia/cocoa-ripeness-dataset-tcs-01

### Karakteristik Dataset

* Dataset tidak dipisahkan ke dalam folder kelas
* Label kelas ditentukan berdasarkan **nama file**:

  * `I#` → Immature (Mentah)
  * `M#` → Mature (Matang)
  * `S#` → Overmature (Mentah)
* Untuk keperluan klasifikasi biner:

  * **Mentah = Immature + Overmature**
  * **Matang = Mature**

---

## Distribusi Dataset

### Split Dataset

| Split      | Jumlah Gambar |
| ---------- | ------------- |
| Train      | 333           |
| Validation | 71            |
| Test       | 72            |
| **Total**  | **476**       |

### Distribusi Kelas (Test Set)

| Kelas  | Jumlah |
| ------ | ------ |
| Mentah | 40     |
| Matang | 32     |

Class imbalance ditangani menggunakan **class weighting** saat training.

---

## Preprocessing & Augmentasi

### Preprocessing

* Resize image ke **224 × 224**
* Normalisasi pixel ke rentang **0–1**
* Konversi label berdasarkan nama file

### Augmentasi Data

* Random Horizontal Flip
* Random Rotation (±15°)
* Random Zoom
* Random Contrast
* Random Brightness

Augmentasi diterapkan **hanya pada data training** untuk meningkatkan generalisasi model.

---

## Pipeline Pengembangan Model

Pengembangan model dilakukan secara bertahap dengan pipeline berikut:

1. Data Understanding
2. Dataset Preparation & Label Parsing
3. Dataset Splitting (Train / Validation / Test)
4. Preprocessing & Augmentation
5. Model Building (Transfer Learning)
6. Training Stage 1 (Feature Extraction)
7. Training Stage 2 (Fine-Tuning)
8. Model Evaluation
9. Model Saving
10. Inference & Visualization

<img width="1024" height="572" alt="image" src="https://github.com/user-attachments/assets/8db9f187-524a-46b8-8821-5fa1eb5a75c5" />


---

## Arsitektur Model

Model menggunakan **MobileNetV2** sebagai feature extractor dengan konfigurasi:

* Input Shape: `(224, 224, 3)`
* Data Augmentation Layer
* MobileNetV2 (pretrained ImageNet)
* Global Average Pooling
* Dense Layer (128 units, ReLU)
* Dropout
* Output Layer (1 unit, Sigmoid)

### Ringkasan Parameter

* **Total parameter**: ±2.42 juta
* **Trainable parameter (Stage 1)**: ±164 ribu
* **Trainable parameter (Stage 2 Fine-Tuning)**: ±1.96 juta

---

## Training Strategy

### Stage 1 – Feature Extraction

* Backbone MobileNetV2 dibekukan
* Training hanya pada classifier head

### Stage 2 – Fine-Tuning

* Layer MobileNetV2 dibuka sebagian (mulai layer ke-107)
* Learning rate diturunkan untuk stabilitas

### Konfigurasi Training

* Optimizer: Adam
* Loss Function: Binary Crossentropy
* Metrics: Accuracy, Precision, Recall, AUC

Callback yang digunakan:

* EarlyStopping
* ReduceLROnPlateau

---

## Evaluasi Model

Evaluasi dilakukan menggunakan **Test Set** yang tidak pernah digunakan selama proses training.

### Hasil Evaluasi Akhir (Stage 2)

| Metric    | Nilai      |
| --------- | ---------- |
| Accuracy  | **96%**    |
| Precision | **96%**    |
| Recall    | **96%**    |
| AUC       | **0.9883** |

### Classification Report

| Class  | Precision | Recall | F1-Score |
| ------ | --------- | ------ | -------- |
| Mentah | 0.93      | 1.00   | 0.96     |
| Matang | 1.00      | 0.91   | 0.95     |

---

## Visualisasi Evaluasi Model

Visualisasi evaluasi model meliputi:

* Prediction Probability Distribution
* ROC Curve & AUC
* Confusion Matrix

<img width="558" height="490" alt="image" src="https://github.com/user-attachments/assets/0ce4ad93-3a5e-443e-a236-72f1d02bcba9" />

<img width="590" height="490" alt="image" src="https://github.com/user-attachments/assets/07049f73-9af7-4a71-a389-b41dc55ac9bc" />

<img width="590" height="490" alt="image" src="https://github.com/user-attachments/assets/d7a76b49-dd95-4989-b7b4-8a07659dd17e" />

---

## Inference Pipeline

Model dapat digunakan untuk inferensi pada gambar baru dengan alur:

1. Load model `.keras`
2. Preprocessing image
3. Prediction
4. Output label dan confidence score

### Class Mapping

```json
{
  "0": "Mentah",
  "1": "Matang"
}
```

---

## Kesimpulan

Model MobileNetV2 dengan pendekatan transfer learning dan fine-tuning berhasil mengklasifikasikan kematangan buah kakao dengan performa yang sangat baik.
Model bersifat **ringan**, **akurat**, dan **siap untuk deployment** pada perangkat mobile maupun edge computing.
