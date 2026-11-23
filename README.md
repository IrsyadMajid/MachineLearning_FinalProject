# Prediksi Tingkat Risiko Kesehatan Ibu Hamil (Maternal Health Risk)

Final Project Machine Learning yang bertujuan untuk memprediksi tingkat risiko kesehatan ibu hamil (*low risk*, *mid risk*, *high risk*) menggunakan algoritma **K-Nearest Neighbors (KNN)**.

---

## 📊 Dataset Overview
- **Dataset Asli**: `Maternal Health Risk Data Set.csv`
- **Dataset Bersih**: `Maternal_Health_Clean.csv`
- **Fitur / Atribut**:
  1. `Age` : Usia ibu hamil (tahun)
  2. `SystolicBP` : Tekanan darah sistolik (mmHg)
  3. `DiastolicBP` : Tekanan darah diastolik (mmHg)
  4. `BS` : Kadar gula darah / Blood Sugar (mmol/L)
  5. `BodyTemp` : Suhu tubuh (°F)
  6. `HeartRate` : Detak jantung (bpm)
  7. `RiskLevel` : Tingkat risiko kesehatan (`low risk`, `mid risk`, `high risk`) — *Target*

---

## 🧹 Ringkasan Proses Data Cleaning

Proses audit dan pembersihan data dilakukan pada notebook [`Preprocessing_Maternal_Health.ipynb`](Preprocessing_Maternal_Health.ipynb) sebelum data digunakan pada tahapan pemodelan.

### 1. Temuan Audit Kualitas Data
Sebelum proses pembersihan, dilakukan audit kualitas data dengan ringkasan temuan sebagai berikut:
- **Total Baris Awal**: 1.014 baris
- **Missing Values**: 0 (tidak ditemukan nilai null/kosong)
- **Exact Duplicates**: 562 baris (55.42%)
- **Konflik Label**: 35 kombinasi fitur identik namun memiliki label `RiskLevel` berbeda (melibatkan **215 baris**)
- **Anomali Medis**:
  - `HeartRate < 40 bpm`: 2 baris (terdapat nilai 7 bpm yang secara medis tidak valid)
  - `Age < 13 tahun`: 39 baris
  - `Age > 60 tahun`: 8 baris

### 2. Langkah-Langkah Cleaning
1. **Pembersihan Anomali Medis (`HeartRate`)**:
   Menghapus 2 baris dengan `HeartRate < 40` bpm karena merupakan anomali/kesalahan input fisiologis yang tidak valid.
2. **Pembersihan Baris Konflik Label**:
   Menghapus semua baris (215 baris) yang memiliki nilai fitur identik namun label target bertentangan. Baris ambigu ini dieliminasi agar tidak merusak pembelajaran model (khususnya KNN yang berbasis jarak).
3. **Pembersihan Exact Duplicates**:
   Menghapus baris duplikat yang tersisa (100% identik di seluruh kolom) dengan mempertahankan kemunculan pertama.

### 3. Hasil Akhir Data Cleaning
| Metrik | Sebelum Cleaning | Setelah Cleaning |
| :--- | :---: | :---: |
| **Jumlah Baris** | 1.014 | **380** |
| **Jumlah Kolom** | 7 | **7** |
| **Missing Values** | 0 | **0** |
| **Exact Duplicates** | 562 | **0** |
| **Konflik Label Tersisa** | 35 kombinasi (215 baris) | **0** |
| **HeartRate < 40 Tersisa** | 2 | **0** |
| **Persentase Data Dipertahankan** | 100% | **37.48%** |

### 4. Perbandingan Distribusi Target (`RiskLevel`)
| Kelas Target | Sebelum Cleaning (1.014 baris) | Setelah Cleaning (380 baris) |
| :--- | :---: | :---: |
| **Low Risk** | 406 (40.04%) | **203 (53.42%)** |
| **High Risk** | 272 (26.82%) | **101 (26.58%)** |
| **Mid Risk** | 336 (33.14%) | **76 (20.00%)** |

Data yang telah bersih disimpan ke file **[`Maternal_Health_Clean.csv`](Maternal_Health_Clean.csv)** dan siap untuk tahap *Stratified Train-Test Split* serta eksperimen model KNN.
