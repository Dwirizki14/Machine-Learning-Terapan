# Sistem Rekomendasi Drama Korea

## Project Overview
Industri hiburan Korea Selatan, khususnya drama Korea (K-Drama), mengalami pertumbuhan signifikan dalam satu dekade terakhir. Menurut Korea Creative Content Agency (KOCCA), ekspor konten drama Korea mencapai lebih dari 500 juta USD pada tahun 2023, menunjukkan peningkatan minat global.

Namun, banyaknya judul dan genre membuat pengguna kesulitan memilih tontonan yang sesuai. Sistem rekomendasi hadir untuk membantu pengguna menemukan konten yang relevan, menghemat waktu, dan mendukung platform streaming menyusun rekomendasi untuk pengguna baru (cold start).

Proyek ini membangun sistem rekomendasi drama Korea menggunakan pendekatan **Content-Based Filtering**, dengan fokus pada fitur **genre**, serta membandingkan performa antara metode **TF-IDF + Cosine Similarity** dan **K-Nearest Neighbors (KNN)**.

Sistem ini penting untuk dikembangkan karena dapat:

- Membantu pengguna menemukan drama Korea baru yang sesuai dengan minat mereka,

- Mengurangi waktu pencarian dan eksplorasi konten secara manual,

- Memberikan insight kepada platform streaming atau penyedia konten dalam menyusun rekomendasi untuk pengguna baru (cold start).

  Riset sebelumnya menunjukkan bahwa Content-Based Filtering efektif diterapkan dalam sistem rekomendasi berbasis metadata konten seperti genre, sinopsis, atau direktur produksi (Lops et al., 2011). Dengan menggunakan pendekatan seperti TF-IDF vectorization dan cosine similarity, proyek ini akan menunjukkan bagaimana preferensi berdasarkan konten dapat dijadikan dasar sistem rekomendasi yang efisien dan relevan.
---

##  1. Business Understanding

###  Problem Statements
1. Bagaimana cara merekomendasikan drama Korea kepada pengguna berdasarkan preferensi genre mereka?
2. Bagaimana mengukur kemiripan antar drama berdasarkan informasi genre yang tersedia?

###  Goals
- Membangun sistem rekomendasi drama Korea berdasarkan genre.
- Menerapkan teknik pemrosesan teks seperti TF-IDF dan cosine similarity untuk menghitung kemiripan antar drama berdasarkan genre.
- Menguji dan membandingkan hasil rekomendasi dari dua pendekatan berbeda: TF-IDF + Cosine Similarity dan K-Nearest Neighbors (KNN).

### Solution Approach
- Menggunakan *TF-IDF Vectorizer* untuk mengubah informasi genre menjadi representasi numerik.
- Menghitung *cosine similarity* antar drama berdasarkan representasi TF-IDF.
- Membuat fungsi rekomendasi untuk menyarankan drama yang mirip berdasarkan input pengguna.
---

##  2. Data Understanding

- **Dataset**: [Top 250 Korean Dramas – Kaggle](https://www.kaggle.com/datasets/ahbab911/top-250-korean-dramas-kdrama-dataset)

Dataset berisi informasi lengkap dari 250 drama Korea teratas. Berikut adalah penjelasan masing-masing fitur:

###  Fitur dalam Dataset

| Fitur                  | Tipe Data  | Deskripsi                                  |
|------------------------|------------|---------------------------------------------|
| `Name`                 | object     | Judul drama                                 |
| `Aired Date`           | object     | Tanggal penayangan                          |
| `Year of release`      | int64      | Tahun rilis                                 |
| `Original Network`     | object     | Jaringan TV                                 |
| `Aired On`             | object     | Hari penayangan                             |
| `Number of Episodes`   | int64      | Jumlah episode                              |
| `Duration`             | object     | Durasi tiap episode                         |
| `Content Rating`       | object     | Rating umur penonton                        |
| `Rating`               | float64    | Skor rating penonton                        |
| `Synopsis`             | object     | Ringkasan cerita                            |
| `Genre`                | object     | Daftar genre (dipisahkan koma)              |
| `Tags`                 | object     | Tag atau keyword tambahan                   |
| `Director`             | object     | Sutradara utama                             |
| `Screenwriter`         | object     | Penulis skenario                            |
| `Cast`                 | object     | Aktor dan aktris utama                      |
| `Production companies` | object     | Rumah produksi                              |
| `Rank`                 | object     | Peringkat dalam dataset                     |

###  Kondisi Data (Missing Values)
- Content Rating: 5
- Director: 1
- Screenwriter: 1
- Production companies: 2

### Duplikasi
- Tidak ada data duplikat pada dataset tersebut

### Menghapus kolom tidak relevan:
   - 'Aired Date', 'Original Network', 'Aired On',
   'Number of Episodes', 'Duration', 'Content Rating',
   'Director', 'Screenwriter', 'Cast', 'Production companies', 'Rank'
   
   - Tujuannya:
   Menyederhanakan data hanya ke kolom-kolom yang benar-benar penting untuk sistem rekomendasi berbasis konten (content-based)
---

##  Visualisasi & Insight

###  Distribusi Rating Drama Korea
- Sebagian besar drama mendapatkan rating antara 8.2 hingga 8.6, yang menunjukkan kualitas produksi yang stabil dan cukup tinggi.

- Hanya sedikit drama yang mencapai rating > 9, menunjukkan eksklusivitas drama yang benar-benar luar biasa di mata penonton.

- Distribusi condong ke kiri, artinya sebagian besar drama dikurasi dari yang terbaik dalam dataset (bukan dari populasi umum).

###  Jumlah Drama Korea Berdasarkan Tahun Rilis
- Terlihat peningkatan yang signifikan sejak tahun 2012, dengan puncak produksi terjadi pada tahun 2021.

- Tahun 2019–2021 menjadi masa paling produktif, dengan jumlah rilis mendekati atau melebihi 35 drama per tahun.

- Setelah 2021, sedikit penurunan di 2022, kemungkinan dipengaruhi oleh transisi pasca-pandemi COVID-19 atau kriteria kurasi data yang hanya mencakup drama terkenal.

- Tren ini menunjukkan bahwa industri drama Korea semakin berkembang dan mendapat perhatian global dalam dekade terakhir.


###  Distribusi 10 Genre Drama Korea Terpopuler
- Romance (18.6%) dan Drama (17.7%) mendominasi genre, mencerminkan karakteristik khas K-Drama yang fokus pada hubungan emosional dan dinamika kehidupan.

- Genre seperti Comedy (12.1%), Mystery (12.3%), dan Thriller (11.8%) juga populer, menunjukkan bahwa variasi tema mulai berkembang.

- Genre minor seperti Fantasy, Historical, dan Melodrama memiliki pangsa yang lebih kecil, tapi tetap penting sebagai pelengkap diferensiasi konten.

---

##  3. Data Preparation

1. **Menghapus kolom tidak relevan**: Kolom seperti 'Aired Date', 'Original Network', 'Aired On',
   'Number of Episodes', 'Duration', 'Content Rating',
   'Director', 'Screenwriter', 'Cast', 'Production companies', 'Rank' dihapus karena tidak digunakan dalam pemodelan.
2. **Menghapus baris dengan Genre kosong**: Baris yang tidak memiliki informasi genre dihapus karena genre merupakan fitur utama untuk sistem rekomendasi.
3. **Mengubah `Genre` menjadi list**: Genre yang awalnya dalam bentuk string (dipisahkan koma) dikonversi menjadi list agar bisa digunakan dalam proses encoding.
4. **Encoding Genre**:
   - Menggunakan `MultiLabelBinarizer` untuk membuat representasi biner dari genre (digunakan oleh model KNN).
   - Menggunakan `TF-IDF Vectorizer` untuk menghasilkan matrix genre dalam bentuk vektor numerik (digunakan oleh model TF-IDF + Cosine Similarity).

##  4. Modeling

### Metode 1: TF-IDF + Cosine Similarity
- Membangun vektor genre dengan TF-IDF.
- Menghitung kemiripan antar drama menggunakan `cosine_similarity`.

Contoh Rekomendasi untuk "SKY Castle":
- My Mister 
- The Penthouse 
- Beyond Evil
- The Penthouse 2
- Hello Monster 

### Metode 2: K-Nearest Neighbors (KNN)
- Menggunakan hasil encoding genre dari `MultiLabelBinarizer`.
- Membangun model KNN berdasarkan jarak antar genre.

Contoh Rekomendasi untuk "SKY Castle":
- My Mister
- The Penthouse 2
- The Penthouse
- Beyond Evil
- Hello Monster


---

##  5. Evaluation


### Metrik Evaluasi: **Precision@K**

Precision@K mengukur seberapa banyak dari K drama yang direkomendasikan benar-benar relevan dengan preferensi pengguna (genre yang mirip dengan input).

#### 📌 Hasil Evaluasi:

| Model                        | Precision@5 |
|-----------------------------|--------------|
| TF-IDF + Cosine Similarity  | 1.0          |
| K-Nearest Neighbors (KNN)   | 1.0          |

#### 🎯 Penjelasan:
- **TF-IDF + Cosine Similarity** merekomendasikan 5 drama dengan genre yang semuanya relevan dengan drama input “SKY Castle”.
- **KNN** juga memberikan hasil yang identik, menunjukkan bahwa representasi genre dalam bentuk biner juga efektif dalam kasus ini.

> Kedua model sama-sama berhasil memberikan rekomendasi yang tepat berdasarkan genre, dengan Precision@5 mencapai 100%.


---

##  6. Conclusion

Proyek ini berhasil membangun sistem rekomendasi drama Korea berbasis konten (Content-Based Filtering) dengan menggunakan genre sebagai fitur utama. Dua pendekatan model dikembangkan dan dibandingkan, yaitu:

  1. TF-IDF + Cosine Similarity

  2. K-Nearest Neighbors (KNN)

Hasil evaluasi menunjukkan bahwa kedua model mampu memberikan rekomendasi yang relevan dengan preferensi pengguna, dengan Precision@5 mencapai 1.0 untuk kedua pendekatan pada studi kasus "SKY Castle". Hal ini menunjukkan bahwa fitur genre cukup representatif dalam mencerminkan kemiripan antar drama Korea.

Proses yang dilakukan mencakup:

- Pemrosesan data untuk memastikan kualitas input.

- Transformasi teks genre menjadi representasi numerik.

- Penghitungan kemiripan antar drama.

- Pembuatan fungsi rekomendasi yang interaktif.

Insight Tambahan:

- Genre seperti Romance dan Drama merupakan yang paling dominan dalam dataset, mencerminkan preferensi umum penonton.

- Rating rata-rata drama tergolong tinggi, menandakan kualitas konten dalam dataset cukup baik untuk digunakan sebagai dasar rekomendasi.


---
