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
Dataset yang digunakan dalam proyek ini bersumber dari platform <a href="https://www.kaggle.com/">Kaggle</a> dengan judul <a href="https://www.kaggle.com/datasets/lava18/google-play-store-apps">"Google Play Store Apps" </a> yang terdapat 2 file penting yang akan dipakai yaitu `googleplaystore.csv` dan `googleplaystore_user_reviews.csv`. Berikut adalah penjelasan rinci terkait kedua file tersebut :
1.  **googleplaystore.csv**

Dataset ini berisi informasi mendetail mengenai aplikasi yang tersedia di Google Play Store, mulai dari nama aplikasi, kategori, rating, jumlah ulasan, ukuran file, jumlah unduhan, hingga versi Android yang dibutuhkan. File tersedia dalam format CSV dan memiliki ukuran relatif kecil, sekitar 1 MB. Secara keseluruhan, dataset ini mencakup sekitar **10.840 baris data** dengan **13 kolom fitur**. Meskipun demikian, inspeksi awal terhadap dataset menunjukkan adanya beberapa tantangan, seperti nilai kosong pada kolom penting seperti `Rating` dan `Size`, entri duplikat, format numerik yang tidak seragam (misalnya pada kolom `Installs` dan `Price`), serta keberadaan baris-baris yang tidak valid.

Berikut adalah uraian untuk seluruh fitur yang ada pada dataset `googleplaystore.csv` :
| Nama Kolom       | Deskripsi                                                              |
|------------------|------------------------------------------------------------------------|
| `App`            | Nama aplikasi                                                          |
| `Category`       | Kategori tempat aplikasi diklasifikasikan (misalnya: GAME, TOOLS, dll) |
| `Rating`         | Rating rata-rata pengguna (0.0 - 5.0)                                  |
| `Reviews`        | Jumlah ulasan yang diberikan pengguna                                  |
| `Size`           | Ukuran file aplikasi (MB atau KB, bisa juga "Varies with device")      |
| `Installs`       | Jumlah unduhan aplikasi                                                |
| `Type`           | Jenis aplikasi: Gratis (`Free`) atau Berbayar (`Paid`)                 |
| `Price`          | Harga aplikasi (jika berbayar, dalam USD)                              |
| `Content Rating` | Segmentasi umur pengguna aplikasi (Everyone, Teen, dll)                |
| `Genres`         | Genre tambahan dari aplikasi                                           |
| `Last Updated`   | Tanggal terakhir pembaruan aplikasi                                    |
| `Current Ver`    | Versi terbaru dari aplikasi                                            |
| `Android Ver`    | Minimum versi Android yang dibutuhkan                                  |

Beberapa analisis dan visualisasi awal dilakukan untuk memahami karakteristik data : 
1.  Distribusi Rating
-  Rating sebagian besar aplikasi berkisar antara 3.5 hingga 4.5
-  Terdapat nilai `null` pada kolom `Rating` yang perlu diatasi
2.  Jumlah Aplikasi per Kategori
-  Kategori dengan jumlah aplikasi terbanyak adalah **FAMILY**, diikuti oleh **GAME** dan **TOOLS**
-  Beberapa kategori seperti **BEAUTY** dan **PARENTING** hanya memiliki sedikit aplikasi
3.  Tipe Aplikasi (Gratis vs Berbayar)
-  Sekitar 90% aplikasi adalah **Gratis**
-  Aplikasi berbayar cenderung memiliki harga <$10
4.  Jumlah Unduhan (Installs)
-  Terdapat distribusi yang sangat tidak merata
-  Beberapa aplikasi memiliki >1 miliar unduhan, namun banyak juga aplikasi dengan unduhan <1.000
5.  Ukuran Aplikasi (Size)
-  Ukuran aplikasi yang bervariasi dari <1 MB hingga >100 MB
-  Beberapa entri memliki nilai `Varies with device` yang harus diubah atau disesuaikan
6.  Genre Aplikasi
-  Beberapa aplikasi memiliki lebih dari 1 genre
-  Genre **Tools**, **Entertainment**, dan **Education** merupakan yang paling umum

Secara umum, terdapat berbagai temuan awal yang menarik dari hasil analisis eksploratori (EDA), yaitu :
-  Terdapat dominasi aplikasi gratis, menunjukkan peluang besar untuk monetisasi berbasis iklan.
-  Sebagian besar aplikasi memiliki rating tinggi, menunjukkan adanya _bias positif_ dalam penilaian pengguna
-  Jumlah review dan jumlah unduhan bisa digunakan sebagai indikator popularitas dan kepercayaan pengguna
-  Kategori dan genre bisa menjadi fitur penting dalam pendekatan content-based filtering

2.  **googleplaystore_user_reviews.csv**

Dataset `googleplaystore_user_reviews.csv` berisi sebanyak 64.295 data ulasan pengguna terhadap aplikasi yang tersedia di Google Play Store. Terdiri dari lima kolom, yaitu `App`, `Translated_Review`, `Sentiment`, `Sentiment_Polarity`, dan `Sentiment_Subjectivity`, dataset ini memberikan informasi penting terkait persepsi pengguna terhadap aplikasi yang mereka gunakan. Setiap ulasan telah diterjemahkan ke dalam bahasa Inggris dan dikategorikan secara sentimen ke dalam 3 kelompok utama : positif, netral, dan negatif. Nilai `Sentiment_Polarity` dan `Sentiment_Subjectivity` masing-masing menggambarkan seberapa kuat suatu sentimen dan seberapa subjektif opini yang diberikan.

Namun, kondisi data tidak sepenuhnya bersih. Terdapat sejumlah nilai kosong khususnya pada kolom `Translated_Review`, `Sentiment_Polarity`, dan `Sentiment_Subjectivity`, yang dapat memengaruhi kualitas analisis jika tidak ditangani dengan baik. Selain itu, ditemukan juga data duplikat yang harus dihapus untuk menjaga validitas hasil eksplorasi dan model. Beberapa entri aplikasi dalam dataset ini juga tidak memiliki padanan langsung dengan dataset utama (`googleplaystore.csv`), sehingga perlu diselaraskan terlebih dahulu sebelum dilakukan integrasi data lebih lanjut. Dengan pembersihan dan penyelarasan yang tepat, dataset ulasan pengguna ini sangat potensial untuk digunakan dalam sistem rekomendasi berbasis sentimen.

Berikut adalah uraian untuk seluruh fitur yang ada pada dataset `googleplaystore_user_reviews.csv` :
| Fitur                   | Deskripsi                                         |
|-------------------------|---------------------------------------------------|
| `App`                   | Nama aplikasi                                     |
| `Translated_Review`     | Isi ulasan (dalam Bahasa Inggris)                 |
| `Sentiment`             | Kategori sentimen (Positive, Neutral, Negative)   |
| `Sentiment_Polarity`    | Nilai polaritas (-1 sampai 1)                     |
| `Sentiment_Subjectivity`| Nilai subjektivitas (0 = obyektif, 1 = subjektif) |

Berikut adalah EDA penting yang dapat dilakukan :
-  **Distribusi Sentimen** : Distribusi kategori sentimen (`Positive`, `Neutral`, `Negative`) membantu memahami bagaimana perasaan pengguna terhadap aplikasi secara umum
-  **Pola Polaritas Sentimen** : Visualisasi `Sentiment_Polarity` dapat menunjukkan sejauh mana ulasan bersifat sangat positif atau sangat negatif. Hal ini berguna untuk model collaborative filtering berbasis nilai sentimen
-  **Analisis Subjektivitas** : Distribusi nilai `Sentiment_Subjectivity` dapat memberikan wawasan seberapa obyektif atau subjektif ulasan pengguna
-  **Ulasan Populer** : Menampilkan aplikasi dengan jumlah ulasan terbanyak atau sentimen paling ekstrem dapat membantu mengidentifikasi aplikasi populer atau kontroversial.

Secara umum, terdapat berbagai temuan awal yang menarik dari hasil analisis eksploratori (EDA), yaitu :
-  **Ulasan Positif** mendominasi dataset ini, menunjukkan bahwa sebagian besar pengguna merasa puas dengan aplikasi yang mereka gunakan
-  **Sebagian ulasan negatif** justru mengandung nilai polaritas tinggi, menandakan adanya ketidaksesuaian antara kategori sentimen dan nilai polaritas. Ini penting untuk diperiksa lebih lanjut
-  **Distribusi nilai polaritas** relatif simetris dengan kecenderungan sedikit condong ke arah positif
-  Beberapa ulasan yang sama muncul berulang kali untuk aplikasi tertentu (indikasi bahwa data perlu dibersihkan dari duplikasi)

## Data Preparation
Tahap data preparation merupakan langkah penting sebelum membangun sistem rekomendasi, karena kualitas data secara langsung memengaruhi hasil model yang dikembangkan. Dalam proyek ini, dilakukan beberapa teknik dan tahapan data preparation berikut secara berurutan pada masing-masing dataset: 
1.  **Dataset `googleplaystore`**
-  **Menghapus Duplikat** : Beberapa entri aplikasi ditemukan lebih dari satu kali. Data duplikat dihapus menggunakan fungsi `drop_duplicates()` untuk memastikan tiap aplikasi hanya diwakili satu baris unik
-  **Menangani Nilai Kosong** : Kolom seperti `Rating`, `Size`, `Genres`, dan `Current Ver` memiliki missing value. Dan baris dengan nilai kosong pada kolom kritikal seperti `Rating` dihapus.
-  **Konversi Tipe Data** : `Installs` dan `Price` berisi karakter non-numerik seperti `'+'`, `','`, dan `'$'`, yang dibersihkan untuk konversi ke `int` atau `float`
-  **Encoding** : Kolom `Type`, `Category`, dan `Content Rating` diubah menjadi label numerik untuk pemrosesan lebih lanjut

2.  **Dataset `googleplaystore_user_reviews`** 
-  **Menghapus Duplikat** : Beberapa ulasan terduplikasi dihapus untuk menjaga representasi opini pengguna tetap proporsional
-  **Menangani Nilai Kosong** : Baris dengan `NaN` pada `Sentiment`, `Sentiment_Polarity`, dan `Sentiment_Subjectivity` dihapus karena mengganggu proses analisis sentimen
-  **Encoding** : Kolom `Sentiment` (Positive, Neutral, Negative) dikonversi ke label numerik (`1`, `0`, `-1`) agar dapat digunakan pada proses integrasi ke sistem rekomendasi berbasis sentimen.

Tahapan _data preparation_ sangat penting karena : 
-  **Konsistensi dan Kualitas Data** : Duplikasi dan missing values bisa menyebabkan bias atau error dalam proses analisis dan pelatihan model
-  **Kompatibilitas Data dengan Model** : Beberapa algoritma hanya menerima input numerik atau format tertentu. Oleh karena itu, konversi dan encoding diperlukan.
-  **Kesiapan Integrasi antar Dataset** : Dengan struktur data yang seragam dan bersih, kedua dataset bisa digabungkan (merge) untuk membuat sistem rekomendasi yang lebih cerdas, misalnya sistem hybrid berbasis konten dan sentimen pengguna.

Tanpa tahap data preparation yang baik, model yang dikembangkan dapat menghasilkan prediksi yang tidak akurat atau bias. Proses ini juga membantu mengurangi kesalahan selama training, mempercepat waktu komputasi, dan meningkatkan akurasi model. Data yang bersih, terstruktur, dan terstandarisasi akan lebih mudah dianalisis dan diolah oleh algoritma sistem rekomendasi.

## Modelling
