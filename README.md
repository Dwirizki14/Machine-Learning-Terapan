# Machine-Learning-Terapan

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

### Goals
- Membangun model prediktif untuk memperkirakan biaya asuransi pelanggan baru.  
- Memberikan insight fitur penting yang menjadi pertimbangan dalam penetapan premi.

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

### Alasan Data Preparation
- Encoding diperlukan karena algoritma machine learning tidak dapat bekerja langsung dengan fitur kategorik.  
- Standarisasi diperlukan agar fitur numerik memiliki skala yang seragam sehingga model tidak bias terhadap fitur tertentu.  
- Pembagian data bertujuan untuk menguji kemampuan model pada data yang belum pernah dilihat sebelumnya, menghindari overfitting.

---

## 5. Modeling

Tiga algoritma digunakan untuk memodelkan prediksi biaya asuransi:

1. **K-Nearest Neighbor (KNN)**  
   - Kelebihan: mudah dipahami, efektif untuk dataset kecil hingga menengah.  
   - Kekurangan: sensitif terhadap fitur yang tidak distandarisasi, lambat pada dataset besar.

2. **Random Forest**  
   - Kelebihan: mengatasi overfitting dengan ensemble, baik untuk fitur campuran.  
   - Kekurangan: kurang interpretatif dibanding model linear.

3. **Gradient Boosting**  
   - Kelebihan: akurasi tinggi, dapat melakukan tuning hyperparameter yang efektif.  
   - Kekurangan: waktu pelatihan lebih lama dibanding model lain.

---

## 6. Evaluation

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

---

## 7. Kesimpulan dan Rekomendasi

- Dataset insurance cocok untuk proyek regresi dengan ukuran data yang memadai (1338 sampel).  
- Proses data preparation penting untuk meningkatkan kualitas model, terutama encoding dan standarisasi.  
- Gradient Boosting adalah algoritma terbaik untuk memprediksi biaya asuransi dengan akurasi lebih baik dibanding KNN dan Random Forest.  
- Rekomendasi selanjutnya adalah melakukan hyperparameter tuning lebih lanjut pada Gradient Boosting dan mencoba model lain seperti XGBoost atau LightGBM untuk potensi peningkatan akurasi.

---

## 8. Referensi

- Mirichoi0218. "Medical Insurance Cost Dataset." Kaggle, 2018. [https://www.kaggle.com/datasets/mirichoi0218/insurance](https://www.kaggle.com/datasets/mirichoi0218/insurance)  
- Pedregosa, F., et al. "Scikit-learn: Machine Learning in Python." Journal of Machine Learning Research, 2011.  
- James, G., Witten, D., Hastie, T., & Tibshirani, R. "An Introduction to Statistical Learning." Springer, 2013.

---

*Laporan ini disusun oleh [Nama Anda]*

