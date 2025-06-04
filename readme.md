# Sistem Rekomendasi Drama Korea
## Project Overview
Industri hiburan Korea Selatan, khususnya drama Korea (K-Drama), mengalami pertumbuhan yang signifikan dalam satu dekade terakhir dan telah memperoleh basis penggemar global yang besar. Menurut data dari Korea Creative Content Agency (KOCCA), ekspor konten drama Korea mencapai lebih dari 500 juta USD pada tahun 2023, menandakan minat yang terus meningkat terhadap konten ini secara global.

Dengan begitu banyak pilihan judul dan genre yang beragam, pengguna sering kali merasa kewalahan saat ingin memilih drama yang sesuai dengan preferensi mereka. Di sinilah sistem rekomendasi memainkan peran penting dalam membantu pengguna menemukan tayangan yang relevan, menarik, dan sesuai selera mereka.

Proyek ini bertujuan untuk membangun sistem rekomendasi drama Korea menggunakan pendekatan Content-Based Filtering, dengan memanfaatkan fitur genre sebagai dasar analisis. Pendekatan ini dipilih karena tidak memerlukan data pengguna seperti riwayat interaksi, dan cocok diterapkan pada dataset yang belum memiliki data user secara eksplisit.

Sistem ini penting untuk dikembangkan karena dapat:

- Membantu pengguna menemukan drama Korea baru yang sesuai dengan minat mereka,

- Mengurangi waktu pencarian dan eksplorasi konten secara manual,

- Memberikan insight kepada platform streaming atau penyedia konten dalam menyusun rekomendasi untuk pengguna baru (cold start).

Riset sebelumnya menunjukkan bahwa Content-Based Filtering efektif diterapkan dalam sistem rekomendasi berbasis metadata konten seperti genre, sinopsis, atau direktur produksi (Lops et al., 2011). Dengan menggunakan pendekatan seperti TF-IDF vectorization dan cosine similarity, proyek ini akan menunjukkan bagaimana preferensi berdasarkan konten dapat dijadikan dasar sistem rekomendasi yang efisien dan relevan.---

## 🧠 1. Business Understanding

###  Problem Statements
1. Bagaimana cara merekomendasikan drama Korea kepada pengguna berdasarkan preferensi genre mereka?
2. Bagaimana mengukur kemiripan antar drama berdasarkan informasi genre yang tersedia?

###  Goals
- Membangun sistem rekomendasi drama Korea berdasarkan genre.
- Menggunakan teknik *Content-Based Filtering* dengan pendekatan TF-IDF dan cosine similarity.

### Solution Approach
- Menggunakan *TF-IDF Vectorizer* untuk mengubah informasi genre menjadi representasi numerik.
- Menghitung *cosine similarity* antar drama berdasarkan representasi TF-IDF.
- Membuat fungsi rekomendasi untuk menyarankan drama yang mirip berdasarkan input pengguna.
---

##  2. Data Understanding

- Dataset: [Top 250 Korean Dramas Dataset – Kaggle](https://www.kaggle.com/datasets/ahbab911/top-250-korean-dramas-kdrama-dataset)
- Fitur yang digunakan:

Dataset berisi informasi lengkap dari 250 drama Korea teratas. Berikut adalah penjelasan masing-masing fitur:

| Fitur                  | Tipe Data  | Deskripsi                                                                 |
|------------------------|------------|---------------------------------------------------------------------------|
| `Name`                | object     | Judul drama Korea                                                         |
| `Aired Date`          | object     | Tanggal penayangan pertama                                                |
| `Year of release`     | int64      | Tahun rilis drama                                                         |
| `Original Network`    | object     | Jaringan TV yang menayangkan drama                                       |
| `Aired On`            | object     | Hari penayangan                                                           |
| `Number of Episodes`  | int64      | Jumlah total episode                                                      |
| `Duration`            | object     | Durasi tiap episode                                                       |
| `Content Rating`      | object     | Batasan usia penonton                                                     |
| `Rating`              | float64    | Skor rating dari penonton                                                 |
| `Synopsis`            | object     | Ringkasan cerita                                                          |
| `Genre`               | object     | Genre drama (dipisahkan koma)                                             |
| `Tags`                | object     | Tag atau keyword tambahan terkait drama                                  |
| `Director`            | object     | Nama sutradara utama                                                      |
| `Screenwriter`        | object     | Nama penulis skenario                                                     |
| `Cast`                | object     | Daftar aktor dan aktris utama                                             |
| `Production companies`| object     | Rumah produksi yang membuat drama                                         |
| `Rank`                | object     | Peringkat berdasarkan popularitas dalam dataset                          |

## Data Preprocessing
- Untuk cek data kosong
- untuk cek data duplikat
Lalu ada Exploratory Data Analysis untuk melihat visualisasi
---

## ⚙️ 3. Data Preparation

- Encoding genre menggunakan MultiLabelBinarizer

---

##  4. Modeling

- Menghitung **cosine similarity** dari hasil `TF-IDF` antar drama
- Membuat fungsi `kdrama_recommendation(kdrama_name, k=5)` untuk mengambil `k` drama paling mirip berdasarkan skor similarity

## 5. Evaluation
Metrik Evaluasi: Cosine Similarity
Mengukur kemiripan antara dua drama berdasarkan genre yang telah diubah menjadi vektor TF-IDF

Nilai cosine similarity berkisar antara 0 hingga 1

1: Sangat mirip

0: Tidak mirip sama sekali

**Hasil Evaluasi:**
Evaluation
Metrik Evaluasi: Precision@k
Precision@k adalah metrik yang mengukur seberapa banyak rekomendasi yang relevan dari total k rekomendasi teratas yang diberikan oleh sistem.

Dalam konteks proyek ini, precision@k mengukur berapa banyak dari k drama yang direkomendasikan berdasarkan genre yang benar-benar sesuai dengan preferensi pengguna atau drama yang mirip secara genre.

**Cara Kerja**
Sistem merekomendasikan k drama teratas yang paling mirip berdasarkan cosine similarity genre.

Kemudian, precision dihitung dengan membandingkan rekomendasi tersebut dengan daftar drama yang dianggap relevan (misalnya, drama dengan genre sama atau penilaian manual).

## 6. Kesimpulan
Sistem rekomendasi berhasil dibangun dengan pendekatan Content-Based Filtering menggunakan representasi TF-IDF dari data genre dan penghitungan cosine similarity untuk mengukur kemiripan antar drama.

Hasil implementasi menunjukkan bahwa:

Drama yang memiliki genre serupa dapat direkomendasikan secara relevan kepada pengguna.

Kemiripan antar drama diukur secara numerik melalui cosine similarity pada vektor TF-IDF genre.

Model ini efektif untuk memberikan rekomendasi berdasarkan preferensi konten (genre) dan dapat menjadi dasar pengembangan sistem rekomendasi yang lebih kompleks ke depannya.
