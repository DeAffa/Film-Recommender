# Laporan Proyek Machine Learning - Muhammad Daffa Nurahman

## Project Overview
Dalam era digital saat ini, industri video game berkembang pesat dengan ribuan judul game baru dirilis setiap tahunnya. Platform distribusi digital seperti Steam menjadi pusat utama bagi para gamer untuk menemukan, membeli, dan memainkan game. Namun, dengan jumlah game yang sangat banyak, pengguna sering mengalami kesulitan dalam menemukan game yang sesuai dengan preferensi mereka. Dalam konteks inilah sistem rekomendasi memainkan peran penting. Sistem rekomendasi membantu menyaring informasi dan memberikan saran produk atau konten yang paling relevan bagi pengguna. Dalam dunia game, sistem ini dapat menyarankan judul game berdasarkan genre, riwayat permainan, rating, serta preferensi pengguna lainnya. 

Dengan sistem rekomendasi yang baik dapat meningkatkan pengalaman pengguna, mempercepat proses penemuan game baru, dan pada akhirnya meningkatkan loyalitas pengguna terhadap platform. Bagi pengembang dan distributor game, sistem rekomendasi juga berperan dalam strategi pemasaran dan peningkatan penjualan. Dengan mengembangkan sistem rekomendasi berbasis hybrid filtering menggunakan dataset dari Steam, proyek ini tidak hanya memberikan solusi terhadap masalah kelebihan informasi (_information overload_), tetapi juga dapat menjadi studi kasus nyata dalam penerapan machine learning dan data mining untuk industri hiburan digital

## Business Understanding
Dengan semakin banyaknya game yang tersedia di platform seperti Steam, pengguna sering kesulitan dalam menemukan game yang sesuai dengan minat dan preferensinya. Meskipun Steam telah menyediakan sistem rekomendasi bawaan, hasil rekomendasi terkadang kurang personal atau lebih bergantung pada popularitas umum. Hal ini menyebabkan pengalaman pengguna tidak optimal, serta potensi game indie atau kurang populer menjadi tidak tereksplorasi. 

Masalah utama yang ingin diselesaikan dalam proyek ini adalah :
-  Bagaimana membangun sistem rekomendasi game yang mampu menyesuaikan saran berdasarkan karakteristik pengguna dan konten game?
-  Bagaimana menggabungkan pendekatan _content-based filtering_ dan _collaborative filtering_ agar rekomendasi lebih relevan dan personal?

Tujuan atau Goals dari proyek ini adalah : 
-  Membangun sistem rekomendasi hybrid yang dapat menyarankan video game berdasarkan preferensi pengguna dan kemiripan konten game
-  Meningkatkan relevansi hasil rekomendasi dengan menggabungkan kekuatan dari dua pendekatan populer : _content-based filteing_ dan _collaborative filtering_
-  Mengurangi kelemahan masing-masing pendekatan, seperti cold-start problem dan overspecialization

Untuk mencapai pada tujuan proyek ini mengusulkan 2 pendekatan umum : 
1.  **Content-Based Filtering** : Pada pendekatan ini, sistem akan merekomendasikan game yang memiliki kemiripan konten dengan game yang pernah disukai atau dimainkan oleh pengguna. Fitur-fitur konten yang akan digunakan antara lain :
-  Genre game (Action, Adventure, RPG, dll)
-  Developer atau publisher

Metode ini akan mengandalkan teknik representasi teks seperti TF-IDF atau CountVectorizer untuk mengubah konten game menjadi vektor numerik, lalu menghitung kemiripan antar game menggunakan _cosine similarity_

2.  **Collaborative Filtering** : Pendekatan ini menggunakan interaksi antar pengguna (misalnya rating atau ulasan) untuk merekomendasikan game. Sistem ini bekerja berdasarkan kesamaan antara pengguna (user-based) atau antara item (item-based). Karena data Steam biasanya berisi review atau waktu bermain (playtime), pendekatan ini dapat memanfaatkan matrix user-item interaksi, lalu menerapkan teknik seperti :
-  **Matrix Factorization** (misalnya SVD)
-  **K-Nearest Neighbors (KNN)** untuk mencari user/item serupa

## Data Understanding
Dataset yang digunakan dalam proyek ini bersumber dari platform <a href="https://www.kaggle.com/">Kaggle</a> dengan judul <a href="https://www.kaggle.com/datasets/lava18/google-play-store-apps">"Steam Store Games Dataset" </a>. Dataset ini menyajikan informasi metadata dari lebih dari 27.000 game yang tersedia di platform distribusi digital Steam, yang mencakup berbagai aspek seperti nama game, tanggal rilis, genre, developer, publisher, jumlah review, serta estimasi waktu bermain dan jumlah pemilik game.

Secara umum dataset ini terdiri dari 19 kolom fitur yang menggambarkan karakteristik konten game dan respons pengguna. Beberapa kolom penting antara lain `genres`, `categories`, dan `steamspy_tags` yang mencerminkan isi atau tipe game, serta `positive_ratings`, `negative_ratings`, dan `average_playtime` yang merepresentasikan interaksi atau pengalaman pengguna terhadap game tersebut. Dataset ini juga menyertakan data harga(`price`) dan platform pendukung (Windows, Mac, Linux) untuk masing-masing game. Dari hasil observasi awal, beberapa fitur memiliki nilai kosong (missing values) seperti pada kolom `release_date`, `price`, dan `publisher`, serta terdapat fitur-fitur bertipe teks kompleks yang mengandung nilai jamak, misalnya genre dan kategori game yang dipisahkan dengan tanda koma.

Berikut adalah fitur yang ada pada dataset `steam.csv` : 
| Nama Fitur         | Tipe Data | Deskripsi                                    |
|--------------------|-----------|----------------------------------------------|
| `appid`            | Integer   | ID unik untuk tiap game                      |
| `name`             | String    | Nama game                                    |
| `release_date`     | String    | Tanggal rilis game                           |
| `english`          | Integer   | 1 jika game berbahasa Inggris, 0 jika tidak  |
| `developer`        | String    | Nama developer game                          |
| `publisher`        | String    | Nama publisher game                          |
| `platforms`        | String    | Platform yang didukung (Windows, Mac, Linux) |
| `required_age`     | Integer   | Batasan usia minimal                         |
| `categories`       | String    | Kategori (Single-player, Multi-player, dll)  |
| `genres`           | String    | Genre game (Action, RPG, Strategy, dll)      |
| `steamspy_tags`    | String    | Tag dari pengguna berdasarkan SteamSpy       |
| `achievements`     | Integer   | Jumlah achievement                           |
| `positive_ratings` | Integer   | Jumlah review positif dari user              |
| `negative_ratings` | Integer   | Jumlah review negatif dari user              |
| `average_playtime` | Integer   | Rata-rata waktu bermain (menit)              |
| `median_playtime`  | Integer   | Median waktu bermain (menit)                 |
| `price`            | Float     | Harga game dalam USD                         |
| `owners`           | String    | Estimasi jumlah pemilik game                 |

Berikut adalah beberapa tahapan eksplorasi data dan visualisasi : 
1.  **Distribusi Genre Game** : Dilakukan analisis frekuensi terhadap kolom `genres` untuk mengetahui jenis game yang paling umum tersedia di platform Steam. Visualisasi menggunakan bar chart
2.  **Distribusi Harga Game** : Histogram digunakan untuk melihat persebaran harga game. Data harga juga ditelusuri untuk melihat proporsi game yang gratis (free-to-play)
3.  **Distribusi Rating Positif dan Negatif** : Scatter plot atau histogram digunakan untuk menggambarkan hubungan antara `positive_ratings` dan `negative_ratings`
4.  **Rata-rata Waktu Bermain (Playtime)** : Histogram dan boxplot digunakan untuk melihat sebaran waktu bermain rata-rata (`average_playtime`), serta mencari outlier seperti game dengan waktu main tinggi
5.  **Korelasi Antar Fitur Numerik** : Heatmap korelasi digunakan untuk melihat hubungan antar variabel seperti `price`, `playtime`, dan `ratings`

Dan berikut adalah beberapa temuan atau insight yang didapat dari hasil eksplorasi data : 
1.  Genre game paling banyak adalah **Action**, **Indie**, dan **Adventure**, menunjukkan preferensi umum pengguna dan keragaman genre yang tinggi
2.  Sebagian besar game memiliki harga dibawah $20 dan banyak juga yang berstatus gratis. Ini menjadi faktor penting dalam filtering rekomendasi
3.  Game denga review negatif tinggi seringkali juga memiliki review positif tinggi, menunjukkan popularitas tidak selalu berarti kualitas bagus bagi semua pemain
4.  Sebagian besar game dimainkan dalam waktu yang relatif singkat, namun terdapat outlier dengan waktu main sangat panjang (contohnya game multiplayer kompetitif seperti Dota 2)
5.  Korelasi antara harga dan playtime cukup rendah, menandakan bahwa mahalnya game tidak menjamin tingkat keterlibatan pengguna

## Data Preparation
Tahap _data preparation_ merupakan proses penting yang bertujuan untuk memastikan data dalam kondisi siap digunakan untuk pemodelan sistem rekomendasi. Dataset Steam yang digunakan masih dalam kondisi mentah, sehingga perlu dilakukan beberapa langkah pemberishan dan transformasi data sebelum masuk ke tahap selanjutnya. Proses ini juga memastikan bahwa data sesuai untuk digunakan baik pendekatan **Content-Based Filtering (CBF)** maupun **Collaborative Filtering (CF)**.

Berikut ini adalah tahapan _data preparation_ yang dilakukan, beserta penjelasan dan alasannya : 
1.  **Menghapus Duplikasi dan Data Kosong yang Tidak Relevan** : Langkah pertama adalah menghapus data duplikat dan entri yang memiliki nilai kosong (missing values) pada kolom-kolom penting seperti `genres`, `positive_ratings`, dan `average_playtime`. Kolom-kolom ini sangat berpengaruh pada kedua pendekatan yang digunakan dalam proyek ini. Data yang tidak lengkap dapat mengurangi akurasi sistem rekomendasi dan menyebabkan bias selama pelatihan model
2.  **Pembersihan dan Normalisasi Kolom Teks** : Kolom `genres`, `categories`, dan `steamspy_tags` merupakan fitur teks yang perlu diproses terlebih dahulu. Pada tahap ini dilakukan normalisasi sederhana seperti konversi ke huruf kecil dan penghapusan spasi ekstra. Hal ini penting untuk memastikan konsistensi format dan mempermudah proses ekstraksi fitur konten nantinya, terutama saat menggunakan metode TF-IDF pada pendekatan CBF
3.  **Penambahan Fitur Boolean** : Untuk memperkaya data, ditambahkan fitur baru bernama `is_free` yang merepresentasikan apakah game tersebut gratis atau tidak. Fitur ini diambil berdasarkan nilai harga (`price`). Informasi ini dapat membantu model memahami preferensi pengguna terhadap game gratis dan berbayar, serta memungkinkan penggunaan sebagai fitur filter tambahan pada sistem rekomendasi
4.  **Normalisasi Fitur Numerik** : Fitur numerik seperti `positive_ratings`, `negative_ratings`, `average_playime`, dan `price` memiliki rentang nilai yang sangat bervariasi. Oleh karena itu, dilakukan proses normalisasi untuk menyamakan skala data. Normalisasi ini penting agar model tidak bias terhadap fitur yang memiliki skalah lebih besar, terutama pada pendekatan CF yang bergantung pada kesamaan antar item atau pengguna
5.  **Ekstraksi Fitur Genre Menggunakan TF-IDF** : Untuk pendekatan COntent-Based FIltering, dilakukan ekstraksi fitur dari kolom `genres` menggunakan teknik TF-IDF (Term Frequency-Inverse Document Frequency). Hasil dari proses ini adalah representasi vektor dari genre yang memungkinkan perhitungan kemiripan antar game berdasarkan kontennya. Teknik ini dipilih karena cukup efektif dalam menangkap informasi penting dari data berbasis teks
6.  **Simulasi Interaksi Pengguna untuk Collaborative FIltering** : Karena tidak tersedia data eksplisit interaksi pengguna dalam dataset ini, dilakukan pendekatan simulasi dengan membuat kombinasi pengguna dan game menggunakan `positive_ratings` sebagai proksi dari rating atau preferensi. Dataset kemudian disiapkan dalam format user-item matrix untuk digunakan dalam pendekatan CF. Ini memungkinkan sistem melakukan rekomendasi berdasarkan pola pengguna lain yang menunjukkan kesamaan perilaku


## Modelling
