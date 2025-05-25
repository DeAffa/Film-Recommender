# Laporan Proyek Machine Learning - Muhammad Daffa Nurahman

## Project Overview
Dengan semakit pesatanya perkembangan teknologi dan penetrasi smartphone, jumlah aplikasi yang tersedia di toko aplikasi seperti Google Play Store terus meningkat secara signifikan. Menurut <a href="https://www.statista.com/statistics/276623/number-of-apps-available-in-leading-app-stores/">Statista </a>, pada tahun 2024, jumlah aplikasi di Google Play Store telah melampaui 3 juta. Banyaknya pilihan ini membuat pengguna kesulitan menemukan aplikasi yang sesuai dengan kebutuhan dan preferensi mereka.
Salah satu solusi untuk mengatasi mesalah ini adalah membangun sistem rekomendasi aplikasi. Sistem ini membantu pengguna menemukan aplikasi yang relevan berdasarkan ketertarikan atau perilaku pengguna menemukan aplikasi yang relevan berdasarkan ketertarikan atau perilaku pengguna lain. Sistem rekomendasi telah terbukti sangat efektif dalam berbagai platform digital seperti Netflix, Amazon, dan Spotify, dan kini menjadi kebutuhan penting juga dalam ekosistem aplikasi mobile.

Tanpa sistem rekomendasi yang efektif, pengguna beresiko mengalami informasi yang berlebihan yang berdampak pada menurunnya pengalaman pengguna, waktu pencarian aplikasi yang lebih lama, dan potensi aplikasi bagus yang tidak pernah ditemukan.Proyek ini penting karena dapat membantu meningkatkan pengalaman pengguna dengan menyajikan rekomendasi yang lebih relevan, sekaligus mendukung pengembang aplikasi dalam menjangkau audiens yang tepat secara lebih efisien. Namun, membangun sistem rekomendasi yang akurat dan efisien bukanlah hal yang mudah. Pendekatan content-based filtering hanya mempertimbangkan atribut aplikasi, sementara collaborative filtering membutuhkan data interaksi pengguna yang kadang tidak tersedia. Oleh karena itu, pendekatan **hybrid filtering**, yang menggabungkan keunggulan keduanya, menjadi alternatif yang menarik untuk dikembangkan.

## Business Understanding
Di era digital saat ini, pengguna smartphone dihadapkan pada jutaan aplikasi yang tersedia di platform seperti Google Play Store. Hal ini menyebabkan tantangan besar dalam menemukan aplikasi yang sesuai dengan preferensi, kebutuhan dan minat pengguna. Tanpa bantuan sistem yang cerdas, pengguna harus secara manual mencari dan menyeleksi aplikasi, yang dapat memakan waktu, tidak efisien, dan mengarah pada pengalaman pengguna yang buruk. Beberapa masalah yang diidentifikasi adalah :
-  Fitur pencarian bawaan pada toko aplikasi seringkali tidak memberikan hasil yang personal
-  Pengguna baru (cold-start users) tidak mendapatkan rekomendasi yang akurat karena tidak memiliki riwayat interaksi
-  Aplikasi berkualitas rendah bisa mendapatkan visibilitas lebih tinggi daripada aplikasi yang lebih relevan secara personal

Tujuan utama dari proyek ini adalah untuk mengembangkan sistem rekomendasi aplikasi mobile yang mampu memberikan saran aplikasi kepada pengguna berdasarkan : 
-  Kesamaan konten aplikasi
-  Preferensi dan perilaku pengguna lain terhadap aplikasi tertentu

Secara khusus proyek ini bertujuan untuk :
-  Meningkatkan kualitas rekomendasi aplikasi yang diberikan kepada pengguna
-  Mengatasi masalah cold-start dengan memanfaatkan informasi konten aplikasi
-  Menerapkan pendekatan hybrud filtering yang menggabungkan content-based collaborative filtering untuk memperoleh hasil rekomendasi yang lebih akurat dan personal

Untuk mencapai seluruh tujuan diatas, proyek ini akan mengembangkan dua pendekatan utama yang kemudian dapat digabungkan sebagai sistem hybrid : 
1.  **Content-Based Filtering** : Pendekatan ini merekomendasikan aplikasi berdasarkan kemiripan fitur antar aplikasi. Sistem akan menganalisis deskripsi, genre, dan kategori dari aplikasi yang pernah dilihat atau disukai pengguna, lalu mencari aplikasi lain dengan karakteristik serupa.
2.  **Collaborative Filtering** : Sistem ini akan merekomendasikan aplikasi berdasarkan preferensi pengguna lain yang memiliki pola perilaku serupa. Model ini memanfaatkan data interaksi seperti rating, jumlah review, atau frekuensi unduhan sebagai dasar untuk menemukan kemiripan antar pengguna atau antar aplikasi.

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
