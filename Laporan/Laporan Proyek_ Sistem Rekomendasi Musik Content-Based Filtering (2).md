# Laporan Proyek: Sistem Rekomendasi Musik Content-Based Filtering

## 1. Domain Proyek

### Latar Belakang  
Industri musik digital terus berkembang pesat dengan jutaan lagu baru yang tersedia setiap hari. Dalam konteks ini, sistem rekomendasi berbasis konten menjadi solusi penting untuk personalisasi pengalaman pengguna dengan menganalisis fitur musik seperti genre, tempo, mood, dan karakteristik audio lainnya. Pendekatan ini membantu mengatasi tantangan dalam memberikan rekomendasi lagu yang relevan dan sesuai preferensi pengguna secara individual dalam skala besar (Magadum et al., 2024)[^1].

### Masalah yang Dihadapi  
Meski sistem rekomendasi berbasis konten telah banyak digunakan, pengguna masih mengalami kesulitan dalam menemukan lagu-lagu baru yang benar-benar sesuai dengan selera mereka. Tantangan utama adalah bagaimana mengoptimalkan penggunaan data fitur musik yang kompleks dan variatif untuk menghasilkan rekomendasi yang akurat dan personal, mengingat keragaman preferensi dan dinamika selera musik pengguna.

[^1]: Magadum, H., Azad, H.K., Patel, H. et al. Music recommendation using dynamic feedback and content-based filtering. Multimed Tools Appl 83, 77469–77488 (2024). https://doi.org/10.1007/s11042-024-18636-8

---

## 2. Business Understanding

### Problem Statement  
Bagaimana membangun sistem rekomendasi musik yang dapat memberikan lagu-lagu serupa berdasarkan karakteristik konten lagu guna meningkatkan kepuasan dan keterlibatan pengguna?

### Goals  
Membangun model content-based filtering yang menggabungkan fitur kategorik dan numerik musik untuk menghasilkan rekomendasi lagu yang relevan dan personal.

### Solution Statement  
1. Menggunakan TF-IDF vectorizer untuk fitur kategorik seperti genre dan emosi.  
2. Menggunakan normalisasi MinMaxScaler untuk fitur numerik audio.  
3. Menggabungkan fitur tersebut dan menghitung cosine similarity antar lagu.  
4. Mengimplementasikan fungsi rekomendasi interaktif dengan input judul lagu dan menampilkan daftar lagu serupa.

---

## 3. Data Understanding

### Informasi Data  
Dataset berisi lebih dari 236.988 lagu dari Spotify dengan fitur: `artist`, `song`, `Genre`, `emotion`, `Release Date`, `Key`, `Explicit`, serta fitur numerik seperti `Tempo`, `Energy`, `Danceability`, `Loudness`, dan lainnya.


### Jumlah Data  
- **Dataset awal:** 236.988  baris dan 18 kolom fitur.  
- **Subset data:** 20.000 baris (lagu), 18 kolom.  
- **Subset unik (setelah hapus duplikasi judul lagu):** sekitar 17.977  baris (judul lagu unik).

### Kondisi Data  
- **Missing Values:**  
  Ditemukan 8 baris dengan nilai hilang (missing values) pada kolom `song` .Nilai ini dihapus dari subset yang digunakan untuk menjaga konsistensi dan validitas data karena kolom `song` adalah fitur kunci untuk rekomendasi lagu. 
- **Duplikasi:**  
  Ditemukan beberapa lagu dengan judul yang sama, sehingga dilakukan penghapusan duplikasi judul untuk menghindari ambiguitas dan kesalahan indeks saat membangun sistem rekomendasi.  
- **Outlier:**  
  Fitur numerik seperti tempo, loudness, dan popularity memiliki rentang nilai yang lebar. Namun, outlier tidak dihapus karena karakteristik data audio memang bervariasi secara alami dan penting untuk representasi lagu.  
- **Distribusi Data:**  
  Fitur kategorik seperti genre dan emosi menunjukkan variasi yang cukup luas, mendukung kemampuan sistem untuk merekomendasikan lagu dengan berbagai karakter.


### Variabel/Fitur pada Data  
- **artist**: Nama artis atau grup musik yang membawakan lagu.  
- **song**: Judul lagu.  
- **emotion**: Emosi dominan yang diekstraksi dari lirik menggunakan model bahasa terlatih.  
- **variance**: Ukuran variabilitas pada fitur audio lagu.  
- **Genre**: Genre musik utama lagu.  
- **Release Date**: Tahun rilis lagu.  
- **Key**: Kunci musik lagu.  
- **Tempo**: Kecepatan lagu dalam beats per minute (BPM).  
- **Loudness**: Rata-rata tingkat volume dalam desibel (biasanya bernilai negatif).  
- **Explicit**: Menandakan apakah lagu mengandung konten eksplisit (Ya/Tidak).  
- **Popularity**: Skor popularitas lagu dalam skala 0 sampai 100 berdasarkan jumlah streaming dan faktor lain.  
- **Energy**: Tingkat energi yang dirasakan dalam lagu, skala 0–100.  
- **Danceability**: Seberapa cocok lagu untuk berdansa, skala 0–100.  
- **Positiveness**: Skor positif atau valensi lagu, skala 0–100.  
- **Speechiness**: Indikator keberadaan kata-kata yang diucapkan dalam lagu, skala 0–100.  
- **Liveness**: Kemungkinan lagu direkam secara live, skala 0–100.  
- **Acousticness**: Skor kualitas akustik lagu, skala 0–100.  
- **Instrumentalness**: Kemungkinan lagu bersifat instrumental tanpa vokal, skala 0–100.  


### Sumber Data  
Dataset diunduh dari Kaggle: [200k Spotify Songs Light Dataset](https://www.kaggle.com/datasets/devdope/200k-spotify-songs-light-dataset).

---

## 4. Data Preparation

### Tahapan Data Preparation  

1. **Penanganan Missing Values**  
   Ditemukan 8 baris data dengan nilai kosong (missing values) pada kolom `song`, yang merupakan fitur penting untuk identifikasi lagu. Oleh karena itu, baris-baris tersebut dihapus dari dataset subset untuk menjaga kualitas data dan konsistensi sistem rekomendasi.

2. **Sampling Data**  
   Dataset asli berukuran sangat besar (236.988  lagu), sehingga diambil sampel acak sebanyak 20.000 lagu untuk mempercepat proses pengolahan dan menghindari keterbatasan memori.

3. **Penghapusan Duplikat Judul Lagu**  
   Karena beberapa lagu memiliki judul yang sama, dilakukan penghapusan duplikat berdasarkan kolom `song` sehingga setiap judul lagu menjadi unik. Langkah ini bertujuan menghindari ambiguitas indeks saat membangun sistem rekomendasi.

4. **Pembuatan Fitur Gabungan Kategorik**  
   Fitur kategorik seperti `Genre`, `emotion`, `Key`, dan `Explicit` digabung menjadi satu string gabungan pada tiap baris data. Kolom gabungan ini kemudian diolah menggunakan metode TF-IDF (Term Frequency-Inverse Document Frequency) vectorizer untuk mengubah data teks menjadi representasi numerik yang dapat diproses oleh model.

5. **Normalisasi Fitur Numerik**  
   Fitur numerik audio seperti `Tempo`, `Energy`, `Danceability`, dan lain-lain dinormalisasi menggunakan MinMaxScaler agar semua fitur berada dalam rentang 0 hingga 1. Hal ini bertujuan menghindari dominasi fitur dengan skala besar saat perhitungan similarity.

6. **Penggabungan Fitur Kategorik dan Numerik**  
   Matriks fitur hasil TF-IDF dan matriks fitur numerik hasil normalisasi digabungkan menjadi satu matriks fitur gabungan. Matriks ini menjadi dasar untuk menghitung kemiripan antar lagu menggunakan cosine similarity.

### Alasan Pemilihan Teknik  
- **Penghapusan missing values** pada kolom kunci `song` dilakukan untuk menjaga integritas data karena kolom ini krusial untuk identifikasi lagu dan pencocokan indeks.  
- **Sampling** dilakukan agar proses komputasi lebih efisien dan memungkinkan eksperimen cepat pada subset data yang representatif.  
- **Penghapusan duplikat judul** bertujuan untuk memudahkan pengelolaan indeks dan menghindari ambiguitas pada saat rekomendasi lagu berdasarkan judul.  
- **TF-IDF vectorizer** dipilih karena mampu merepresentasikan informasi tekstual dalam fitur kategorik dengan bobot kata yang mencerminkan relevansi dan frekuensi kemunculan.  
- **MinMaxScaler** digunakan untuk menyamakan skala fitur numerik agar perhitungan similarity menjadi lebih adil dan tidak bias ke fitur tertentu.

---

## 5. Modelling


### Cara Kerja Model  
Model yang digunakan dalam proyek ini adalah **Content-Based Filtering** dengan metode **Cosine Similarity** sebagai ukuran kemiripan antar lagu. Secara singkat, langkah kerja model adalah sebagai berikut:

1. **Representasi Fitur Lagu**  
   Setiap lagu direpresentasikan dalam bentuk vektor fitur yang menggabungkan:  
   - Fitur kategorik yang telah diproses dengan TF-IDF vectorizer untuk mengubah teks (genre, emosi, key, explicit) menjadi vektor numerik.  
   - Fitur numerik audio (tempo, energy, danceability, dan lainnya) yang telah dinormalisasi menggunakan MinMaxScaler agar semua fitur sebanding.

2. **Penghitungan Cosine Similarity**  
   Cosine similarity menghitung sudut kosinus antara dua vektor fitur lagu, menghasilkan nilai antara -1 sampai 1 yang mengukur seberapa mirip kedua lagu tersebut. Nilai 1 menunjukkan kemiripan maksimal, sedangkan 0 menunjukkan tidak ada kemiripan.  
   Formula cosine similarity untuk dua vektor A dan B:  

   $`\text{cosine\_similarity}(A,B) = \frac{A \cdot B}{\|A\| \|B\|}`$

3. **Pemberian Rekomendasi**  
   Setelah matriks kemiripan antar semua lagu dihitung, sistem dapat memberikan rekomendasi Top-N lagu yang paling mirip dengan lagu input berdasarkan nilai cosine similarity tertinggi.

### Alasan Pemilihan Model  
- **Content-Based Filtering** memungkinkan rekomendasi yang personal dan kontekstual berdasarkan isi lagu, tanpa perlu data preferensi pengguna sebelumnya.  
- **Cosine Similarity** efektif dalam mengukur kemiripan vektor berdimensi tinggi, terutama untuk data TF-IDF dan fitur numerik yang telah dinormalisasi.  
- Model ini relatif sederhana, transparan, dan dapat diimplementasikan dengan efisien untuk subset data yang besar.

### Contoh Hasil Top-N Recommendation  

Berikut adalah contoh hasil rekomendasi 5 lagu teratas untuk lagu input "**Baby**" oleh artis **Justin Bieber**:

| No | Artist           | Song                | Genre | Emotion | Similarity |
|----|------------------|---------------------|-------|---------|------------|
| 1  | Backstreet Boys  | All In This Together | pop   | joy     | 0.984      |
| 2  | Ace Of Base      | Life Is A Flower     | pop   | joy     | 0.983      |
| 3  | Vonda Shepard    | Read Your Mind       | pop   | joy     | 0.983      |
| 4  | Backstreet Boys  | Forces Of Nature     | pop   | joy     | 0.982      |
| 5  | Lady Gaga        | Just Dance           | pop   | joy     | 0.980      |

*Catatan: Tabel di atas merupakan output langsung dari sistem rekomendasi musik berdasarkan perhitungan cosine similarity pada fitur gabungan kategori dan numerik.*



---

## 6. Evaluation

### Metrik Evaluasi  
- **Precision@K**: Mengukur proporsi lagu rekomendasi yang relevan (genre sama dengan lagu input) dari total K rekomendasi.  
- **Recall@K**: Mengukur proporsi lagu relevan dalam dataset subset yang muncul dalam rekomendasi top-K.

### Hasil Evaluasi  
- Precision@20 mencapai nilai 1.00 pada contoh lagu uji, menandakan rekomendasi sangat relevan genre.  
- Recall@20 relatif kecil (misalnya 0.00 dan 0.01), karena jumlah lagu relevan di dataset jauh lebih besar daripada jumlah rekomendasi yang diberikan sehingga cakupan recall kecil.

---

## 7. Kesimpulan 

Sistem rekomendasi musik content-based filtering yang dibangun telah berhasil menjawab problem statement yang diajukan dalam Business Understanding, yaitu menyediakan rekomendasi lagu yang relevan dan personal berdasarkan karakteristik konten lagu.

Evaluasi dengan metrik Precision@K dan Recall@K menunjukkan bahwa sistem dapat memberikan rekomendasi dengan tingkat presisi sangat tinggi (Precision@20 mencapai 1.00 pada contoh lagu), sehingga pengguna mendapatkan lagu-lagu yang sangat relevan dengan selera mereka. Meskipun recall masih terbatas, hal ini wajar mengingat jumlah rekomendasi yang diberikan relatif kecil dibanding total lagu relevan dalam dataset.


Setiap solusi yang diterapkan, mulai dari pengolahan fitur kategorik dengan TF-IDF, normalisasi fitur numerik, hingga penggunaan cosine similarity, terbukti efektif dalam menghasilkan rekomendasi yang konsisten secara genre dan karakteristik audio. Dengan demikian, proyek ini berhasil meningkatkan kepuasan dan keterlibatan pengguna pada platform musik digital.

Untuk pengembangan selanjutnya, disarankan untuk memperluas cakupan rekomendasi dengan meningkatkan jumlah lagu yang direkomendasikan (nilai K lebih besar) dan mengintegrasikan data preferensi pengguna guna mengkombinasikan content-based filtering dengan collaborative filtering. Selain itu, penerapan metode approximate nearest neighbors (seperti FAISS atau Annoy) akan memungkinkan sistem skala besar yang efisien untuk dataset lengkap. Penambahan metrik evaluasi lain seperti diversity dan novelty juga akan memperkaya pemahaman terhadap kualitas rekomendasi. Pengembangan antarmuka pengguna yang interaktif dan responsif juga penting untuk meningkatkan pengalaman pengguna secara menyeluruh.

Secara keseluruhan, proyek ini menyediakan fondasi teknis yang kuat untuk sistem rekomendasi musik yang dapat dikembangkan lebih lanjut untuk memenuhi kebutuhan bisnis dan meningkatkan nilai platform musik digital.





