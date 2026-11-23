# ANALISIS KOMPARATIF K-NEAREST NEIGHBORS DAN DECISION TREE PADA KLASIFIKASI RISIKO KESEHATAN IBU HAMIL

Final Project Machine Learning yang bertujuan untuk membandingkan performa algoritma **K-Nearest Neighbors (KNN)** dan **Decision Tree** dalam klasifikasi tingkat risiko kesehatan ibu hamil (*Low*, *High*).

---

## 📊 Dataset Overview
- **Dataset Asli**: `Maternal Risk Data Set.csv`
- **Dataset Bersih**: `Maternal_Risk_Clean.csv`
- **Jumlah Fitur**: 11 fitur prediktor + 1 kolom target (`Risk Level`)
- **Daftar Fitur / Atribut**:
  1. `Age` : Usia ibu hamil (tahun)
  2. `Systolic BP` : Tekanan darah sistolik (mmHg)
  3. `Diastolic` : Tekanan darah diastolik (mmHg)
  4. `BS` : Kadar gula darah / Blood Sugar (mmol/L)
  5. `Body Temp` : Suhu tubuh (°F)
  6. `BMI` : Indeks Massa Tubuh / Body Mass Index (kg/m²)
  7. `Previous Complications` : Riwayat komplikasi kehamilan sebelumnya (0 = Tidak, 1 = Ya)
  8. `Preexisting Diabetes` : Riwayat diabetes sebelum kehamilan (0 = Tidak, 1 = Ya)
  9. `Gestational Diabetes` : Riwayat diabetes gestasional saat hamil (0 = Tidak, 1 = Ya)
  10. `Mental Health` : Riwayat masalah kesehatan mental (0 = Tidak, 1 = Ya)
  11. `Heart Rate` : Detak jantung (bpm)
  12. `Risk Level` : Tingkat risiko kesehatan (`Low`, `High`) — *Target*

---

## 🧹 Ringkasan Proses Data Cleaning

Proses audit dan pembersihan data dilakukan pada notebook [`Preprocessing_Maternal_Risk.ipynb`](Preprocessing_Maternal_Risk.ipynb) sebelum data digunakan pada tahapan pemodelan.

### 1. Temuan Audit Kualitas Data
Sebelum proses pembersihan, dilakukan audit kualitas data dengan ringkasan temuan sebagai berikut:
- **Total Baris Awal**: 1.205 baris
- **Missing Values Total**: 53 nilai kosong
  - `Risk Level` (Target): 18 baris
  - `BMI`: 18 baris
  - `Systolic BP`: 5 baris
  - `Diastolic`: 4 baris
  - `BS`: 2 baris
  - `Heart Rate`: 2 baris
  - `Previous Complications`: 2 baris
  - `Preexisting Diabetes`: 2 baris
- **Exact Duplicates**: 18 baris (1.49%)
- **Konflik Label**: 0 (tidak ditemukan kombinasi fitur identik dengan label berbeda)
- **Anomali Klinis**:
  - `Age > 100`: 1 baris (`Age = 325`, kesalahan input/typo fisiologis)
  - `BMI <= 5`: 1 baris (`BMI = 0.0`, mustahil secara biologis pada manusia hidup)

### 2. Langkah-Langkah Cleaning
1. **Pembersihan Target Missing & Anomali Medis**:
   - Menghapus 18 baris dengan `Risk Level == NaN` karena data tanpa label target tidak dapat digunakan untuk *supervised learning*.
   - Menghapus 1 baris anomali umur ekstrim `Age = 325`.
   - Mengonversi `BMI == 0.0` menjadi `NaN` agar diimputasi dengan nilai yang valid.
2. **Pembersihan Exact Duplicates**:
   - Menghapus 18 baris duplikat identik untuk mencegah bias dan *data leakage*.
3. **Penanganan Missing Values pada Fitur (Imputasi)**:
   - Fitur numerik (`Systolic BP`, `Diastolic`, `BS`, `BMI`, `Heart Rate`) diimputasi menggunakan **Median**.
   - Fitur biner (`Previous Complications`, `Preexisting Diabetes`) diimputasi menggunakan **Modus** (kategori terbanyak).

### 3. Hasil Akhir Data Cleaning
| Metrik | Sebelum Cleaning | Setelah Cleaning |
| :--- | :---: | :---: |
| **Jumlah Baris** | 1.205 | **1.168** |
| **Jumlah Kolom** | 12 | **12** |
| **Missing Values (Total)** | 53 | **0** |
| **Exact Duplicates** | 18 | **0** |
| **Konflik Label Tersisa** | 0 | **0** |
| **Anomali Usia & BMI Tersisa** | 2 | **0** |
| **Persentase Data Dipertahankan** | 100% | **96.93%** |

### 4. Perbandingan Distribusi Target (`Risk Level`)
| Kelas Target | Sebelum Cleaning (1.187 baris berlabel) | Setelah Cleaning (1.168 baris) |
| :--- | :---: | :---: |
| **Low** | 713 (60.07%) | **695 (59.50%)** |
| **High** | 474 (39.93%) | **473 (40.50%)** |

Data yang telah bersih disimpan ke file **[`Maternal_Risk_Clean.csv`](Maternal_Risk_Clean.csv)** dan siap untuk tahap *Stratified Train-Test Split* serta eksperimen perbandingan model KNN dan Decision Tree.
