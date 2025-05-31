# Machine Learning Terapan

# Laporan Proyek Machine Learning - Prediksi Biaya Asuransi Kesehatan

## 1. Domain Proyek
Proyek ini bertujuan untuk memprediksi biaya asuransi kesehatan (charges) berdasarkan data demografis dan karakteristik individu seperti usia, jenis kelamin, indeks massa tubuh (BMI), jumlah anak, kebiasaan merokok, dan wilayah tempat tinggal. Prediksi biaya asuransi penting untuk membantu perusahaan asuransi dalam menetapkan premi yang adil dan untuk membantu individu dalam merencanakan anggaran kesehatan.

Referensi:  
- Chen, C., et al. "Medical Cost Personal Dataset." Kaggle, 2018.  
- Xu, R., & Wunsch, D. "Clustering algorithms in biomedical research: A review." IEEE Reviews in Biomedical Engineering, 2010.

---

## 2. Business Understanding

### Problem Statements
1. Bagaimana memprediksi biaya asuransi kesehatan berdasarkan karakteristik pelanggan?  
2. Fitur apa saja yang paling memengaruhi besarnya biaya asuransi?
3. Bagaimana membangun model prediksi yang akurat dan dapat diandalkan untuk pelanggan baru?

### Goals
- Membangun model prediktif yang mampu menghasilkan prediksi biaya asuransi yang akurat dan dapat dipercaya.
- Membangun model prediktif untuk memperkirakan biaya asuransi pelanggan baru.  
- Menyediakan solusi machine learning yang optimal untuk diaplikasikan pada data pelanggan baru, sehingga memudahkan penetapan premi yang sesuai.

### Solution Statements
- Membangun model baseline menggunakan Linear Regression.
- Melakukan peningkatan model dengan menggunakan algoritma seperti Random Forest Regressor dan XGBoost.
- Melakukan tuning hyperparameter dan evaluasi performa dengan metrik MAE, MSE, dan RMSE.

---

## 3. Data Understanding

Dataset yang digunakan berasal dari Kaggle, berjudul **Medical Insurance Cost Dataset** yang berisi 1338 sampel data. Dataset memiliki 7 fitur, yaitu:

| Fitur    | Tipe       | Deskripsi                                 |
|----------|------------|-------------------------------------------|
| age      | Numerik    | Usia pelanggan (tahun)                    |
| sex      | Kategorik  | Jenis kelamin (male/female)               |
| bmi      | Numerik    | Indeks massa tubuh (Body Mass Index)      |
| children | Numerik    | Jumlah anak yang ditanggung                |
| smoker   | Kategorik  | Apakah pelanggan merokok (yes/no)          |
| region   | Kategorik  | Wilayah tempat tinggal (northeast, ... ) |
| charges  | Numerik    | Biaya asuransi kesehatan (target variabel)|

### Eksplorasi Data

- Data tidak mengandung missing values.  
- Terdapat outliers pada fitur `bmi` dan `charges` yang ditangani menggunakan metode IQR.  
- Distribusi variabel numerik dan kategorik telah dianalisis untuk memahami karakteristik data.

---

## 4. Data Preparation

### Tahapan Data Preparation
1. **Encoding fitur kategori**:  
   Fitur `sex`, `smoker`, dan `region` diubah menjadi variabel dummy (one-hot encoding) agar dapat digunakan dalam model machine learning.

2. **Reduksi dimensi dengan PCA** (opsional):  
   PCA diterapkan untuk mengurangi dimensi data sekaligus mempertahankan variansi terbesar.

3. **Pembagian dataset**:  
   Dataset dibagi menjadi data latih (80%) dan data uji (20%) menggunakan `train_test_split` dengan `random_state=42` agar hasil konsisten.

4. **Standarisasi fitur numerik**:  
   Fitur numerik (`age`, `bmi`, `children`) distandarisasi menggunakan `StandardScaler` agar memiliki mean 0 dan standar deviasi 1. Target variabel `charges` tidak distandarisasi.

5. **Penanganan Outlier**:
   Untuk menghindari pengaruh nilai ekstrem yang dapat merusak performa model, saya melakukan penanganan outlier menggunakan metode Interquartile Range (IQR). Fitur numerik diperiksa menggunakan kuartil 1 (Q1) dan kuartil 3 (Q3). Data di luar [Q1 - 1.5 * IQR, Q3 + 1.5 * IQR] dianggap outlier dan dihapus dari dataset, menghasilkan dataset baru data_clean.

### Alasan Data Preparation
- Encoding diperlukan karena algoritma machine learning tidak dapat bekerja langsung dengan fitur kategorik.  
- Standarisasi diperlukan agar fitur numerik memiliki skala yang seragam sehingga model tidak bias terhadap fitur tertentu.  
- Pembagian data bertujuan untuk menguji kemampuan model pada data yang belum pernah dilihat sebelumnya, menghindari overfitting.
- PCA hanya digunakan untuk eksplorasi tambahan dan tidak digunakan dalam pelatihan model akhir. Model dibangun dengan data hasil encoding dari data_clean yaitu data_encoded.
---

## 5. Modeling

Tiga algoritma digunakan untuk memodelkan prediksi biaya asuransi:

1. **K-Nearest Neighbor (KNN)**
   
   KNN bekerja dengan mencari K tetangga terdekat dari data uji dan mengambil rata-rata target dari tetangga tersebut untuk regresi. saya menggunakan nilai default n_neighbors=5. 
   - Kelebihan: mudah dipahami, efektif untuk dataset kecil hingga menengah.  
   - Kekurangan: sensitif terhadap fitur yang tidak distandarisasi, lambat pada dataset besar.

3. **Random Forest**
   
   Random Forest adalah ensemble dari banyak pohon keputusan (decision tree) yang dibangun secara acak dan hasil prediksi merupakan rata-rata dari semua pohon. Model ini tahan terhadap overfitting dan cocok untuk data non-linear. Parameter penting: n_estimators=100, random_state=42
   - Kelebihan: mengatasi overfitting dengan ensemble, baik untuk fitur campuran.  
   - Kekurangan: kurang interpretatif dibanding model linear.

5. **Gradient Boosting**
   
   Gradient Boosting membangun model secara bertahap dengan menyesuaikan kesalahan model sebelumnya. Ini membuat model sangat akurat namun lebih lambat dilatih. Parameter penting: n_estimators=100, learning_rate=0.1, random_state=42.
   - Kelebihan: akurasi tinggi, dapat melakukan tuning hyperparameter yang efektif.  
   - Kekurangan: waktu pelatihan lebih lama dibanding model lain.

---

## 6. Evaluation

- Feature Importance:
Untuk menjawab fitur mana yang paling memengaruhi biaya asuransi, saya menggunakan metode feature_importances_ dari Random Forest dan Gradient Boosting. Hasil menunjukkan bahwa age, bmi, dan smoker_yes adalah fitur dengan pengaruh terbesar terhadap biaya asuransi.

### Metrik Evaluasi yang Digunakan
- **Mean Absolute Error (MAE)**: rata-rata absolut selisih antara prediksi dan nilai aktual, mengukur kesalahan prediksi secara langsung.  
- **Mean Squared Error (MSE)**: rata-rata kuadrat selisih, memberikan penalti lebih besar untuk kesalahan besar.  
- **Root Mean Squared Error (RMSE)**: akar dari MSE, dalam satuan yang sama dengan target sehingga mudah diinterpretasi.  
- **R-squared (R²)**: proporsi variansi target yang dapat dijelaskan oleh model (nilai 1 adalah model sempurna).

### Hasil Evaluasi

| Model             | MAE       | MSE          | RMSE      | R²       |
|-------------------|-----------|--------------|-----------|----------|
| K-Nearest Neighbor| 2969.30   | 23051721.74  | 4801.22   | 0.52     |
| Random Forest     | 2573.10   | 20426721.24  | 4519.59   | 0.57     |
| Gradient Boosting | 2370.90   | 17879299.93  | 4228.39   | 0.63     |

Dari hasil tersebut, model Gradient Boosting memberikan performa terbaik dengan MAE dan RMSE terendah serta R² tertinggi, menandakan prediksi yang paling akurat dibandingkan model lainnya.

### Hasil Prediksi Model

Berikut adalah hasil prediksi dari tiga model (KNN, Random Forest, dan Gradient Boosting) untuk lima data sampel:

| Index | y_true (Biaya Aktual) | Prediksi KNN     | Prediksi Random Forest | Prediksi Gradient Boosting |
|-------|------------------------|------------------|-------------------------|-----------------------------|
| 660   | 6435.62                | 5875.43          | 8471.37                 | 6173.53                     |
| 754   | 17128.43               | 8873.41          | 5743.82                 | 5920.88                     |
| 487   | 1253.94                | 1597.16          | 1441.00                 | 2042.17                     |
| 429   | 18804.75               | 8880.85          | 7605.73                 | 7965.80                     |
| 559   | 1646.43                | 1832.26          | 2034.42                 | 3287.62                     |

###  Analisis 

- **KNN**

  cenderung memberikan nilai prediksi yang dekat terhadap nilai aktual untuk data dengan nilai y_true yang rendah (contoh: index 487 dan 559), tapi gagal memprediksi data dengan nilai besar (contoh: index 754 dan 429).
  
- **Random Forest**
  
  cenderung underrate (terlalu rendah) untuk nilai besar, dan performanya tidak jauh berbeda dari KNN dalam kasus ini.
  
- **Gradient Boosting**

  memberikan prediksi yang lebih tinggi, namun mendekati target aktual terutama untuk data dengan biaya besar, seperti index 660 dan 429.
---

