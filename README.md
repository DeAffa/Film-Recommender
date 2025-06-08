# Laporan Proyek Machine Learning - Muhammad Daffa Nurahman

## Project Overview
Di era digital saat ini, volume informasi yang tersedia secara daring semakin meningkat secara eksponensial. Hal ini menyebabkan tantangan baru bagi pengguna dalam memilih informasi atau produk yang sesuai dengan preferensi mereka. Untuk mengatasi hal tersebut, sistem rekomendasi (recommender system) telah menjadi komponen penting dalam berbagai platform digital, seperti layanan streaming film (Netflix), e-commerce (Tokopedia, Amazon), hingga media sosial (YouTube, TikTok).Salah satu domain yang paling populer dalam penerapan sistem rekomendasi adalah industri hiburan, khususnya layanan penyedia film. Sistem rekomendasi pada platform ini membantu pengguna menemukan film yang relevan dengan minat mereka tanpa harus mencarinya secara manual. MovieLens, sebagai salah satu dataset paling umum dan banyak digunakan dalam penelitian sistem rekomendasi, menyediakan data rating film yang diberikan oleh pengguna serta metadata film seperti genre dan judul. Dengan menggunakan dataset ini, kita dapat mengeksplorasi berbagai pendekatan sistem rekomendasi, termasuk Collaborative Filtering (CF) dan Content-Based Filtering (CBF).

Pendekatan Collaborative Filtering (CF) bekerja berdasarkan kesamaan pola rating antar pengguna, tanpa melihat isi atau karakteristik item. Sedangkan Content-Based Filtering (CBF) memberikan rekomendasi berdasarkan kemiripan karakteristik antar item, seperti genre, sinopsis, atau tahun rilis film. Menggabungkan kedua pendekatan tersebut dapat menghasilkan sistem rekomendasi yang lebih akurat dan personal, serta mengatasi kekurangan masing-masing metode secara individu. Proyek ini bertujuan untuk membangun sistem rekomendasi film menggunakan pendekatan gabungan CF dan CBF berbasis dataset MovieLens 1M. Dengan menggunakan data yang telah bersih dan terstruktur, proyek ini menjadi peluang yang baik untuk menerapkan teori dan praktik sistem rekomendasi dalam konteks nyata.

## Business Understanding
Dalam dunia digital modern, pengguna dihadapkan pada banyak pilihan ketika ingin menonton film. Platform penyedia film seperti Netflix atau layanan serupa terus menambah koleksi filmnya, namun pengguna tetap membutuhkan bantuan untuk menemukan film yang sesuai dengan selera mereka. Sistem rekomendasi menjadi kunci dalam menyaring informasi ini agar lebih relevan dan personal. Namun demikian, proses rekomendasi tidak lepas dari tantangan teknis dan keterbatasan metode. Pendekatan seperti Collaborative Filtering sangat bergantung pada data interaksi pengguna, sementara pendekatan Content-Based Filtering hanya terbatas pada fitur atau informasi film yang tersedia. Jika tidak ditangani secara tepat, sistem rekomendasi dapat menghasilkan saran yang tidak relevan atau bahkan membingungkan pengguna.

Masalah utama yang ingin diselesaikan dalam proyek ini adalah :
-  Bagaimana menyajikan rekomendasi film yang relevan dan personal kepada pengguna berdasarkan data rating dan metadata film?
-  Bagaimana menangani pengguna baru (cold-start user) yang belum banyak memberikan rating, sehingga sistem tetap dapat memberi saran yang akurat?
-  Bagaimana mengintegrasikan dua pendekatan sistem rekomendasi — Collaborative Filtering dan Content-Based Filtering — agar dapat saling melengkapi kekurangan masing-masing?

Tujuan atau Goals dari proyek ini adalah : 
-  Membangun sistem rekomendasi film yang dapat menyarankan film sesuai dengan preferensi pengguna.
-  Mengimplementasikan dua pendekatan sistem rekomendasi, yaitu:
    -  Collaborative Filtering untuk memanfaatkan pola interaksi antar pengguna dan item.
    -  Content-Based Filtering untuk memanfaatkan informasi konten film seperti genre.
-  Membandingkan efektivitas kedua pendekatan dan/atau menggabungkannya untuk menciptakan sistem rekomendasi hybrid yang lebih akurat dan adaptif.

Untuk mencapai pada tujuan proyek ini mengusulkan 2 pendekatan umum : 
1.  **Content-Based Filtering** : CBF merekomendasikan film kepada pengguna berdasarkan karakteristik dari film yang telah disukai atau diberi rating tinggi oleh pengguna tersebut. Dalam proyek ini, fitur konten yang digunakan adalah genre dari setiap film. Setiap film direpresentasikan sebagai vektor genre, dan sistem menghitung kesamaan antar film (misalnya dengan cosine similarity) untuk menyarankan film yang serupa dengan film yang telah disukai oleh pengguna.

2.  **Collaborative Filtering** : CF merekomendasikan film berdasarkan pola perilaku pengguna lain. Jika dua pengguna memiliki preferensi yang serupa, maka film yang disukai oleh satu pengguna dapat direkomendasikan kepada pengguna lainnya. Dalam proyek ini digunakan pendekatan user-based dan/atau item-based collaborative filtering, berdasarkan data rating dari MovieLens 1M.

Dengan menggabungkan kedua pendekatan ini, diharapkan sistem rekomendasi yang dibangun dapat lebih akurat dan fleksibel dalam memberikan saran film, baik kepada pengguna baru maupun pengguna aktif.

## Data Understanding
Dataset yang digunakan dalam proyek ini bersumber dari platform <a href="https://grouplens.org/">GroupLens Research</a> dengan judul <a href="https://grouplens.org/datasets/movielens/1m/">"MovieLens 1M" </a>. Dataset ini merupakan salah satu dataset populer yang banyak digunakan untuk penelitian dan pengembangan sistem rekomendasi. Dataset MovieLens 1M terdiri dari data rating film yang diberikan oleh sekitar 6.000 pengguna terhadap 4.000 lebih film. Dataset ini terdiri dari tiga file utama dengan struktur sebagai berikut : 
| Nama File     | Deskripsi                                                                                       | Jumlah Baris     |
|---------------|--------------------------------------------------------------------------------------------------|------------------|
| `users.dat`   | Data pengguna, berisi informasi demografis seperti usia, jenis kelamin, pekerjaan, dan kode pos | 6.040 pengguna   |
| `movies.dat`  | Data film, berisi ID film, judul film, dan genre film                                           | 3.952 film       |
| `ratings.dat` | Data interaksi, berisi userId, movieId, rating (1-5), dan timestamp                             | 1.000.209 rating |

Dataset MovieLens 1M yang digunakan dalam proyek ini sudah dalam kondisi yang relatif bersih dan terstruktur dengan baik. Tidak ditemukan nilai yang hilang (missing values) maupun data duplikat yang signifikan pada file pengguna, film, maupun rating. Data rating terdiri dari lebih dari satu juta interaksi antara pengguna dan film, sehingga memberikan cakupan yang cukup luas untuk analisis. Meskipun demikian, dataset ini masih memiliki karakteristik sparsity, di mana sebagian besar pengguna hanya memberikan rating pada sebagian kecil film yang mereka tonton. Hal ini menjadi tantangan khusus dalam pengembangan sistem rekomendasi, terutama untuk pendekatan collaborative filtering. Selain itu, dataset menyediakan informasi genre film yang lengkap dan bervariasi, yang sangat berguna untuk pendekatan content-based filtering. Secara keseluruhan, kondisi data yang sudah bersih dan lengkap ini memungkinkan proses analisis dan pembangunan model rekomendasi dapat dilakukan dengan lebih efisien tanpa harus melakukan tahapan pembersihan data yang kompleks.

Berikut adalah penjelasan variabel utama yang ada pada dataset ini : 
| **Data** | **Nama Fitur** | **Tipe Data**  | **Deskripsi**                        | **Contoh Nilai**            |
|----------|----------------|----------------|--------------------------------------|-----------------------------|
| Users    | `UserID`       | Integer        | Identifikasi unik pengguna           | 1, 2, 3                     |
|          | `Gender`       | Categorical    | Jenis kelamin pengguna (M/F)         | M, F                        |
|          | `Age`          | Categorical    | Kategori usia pengguna (kode umur)   | 18, 25, 35                  |
|          | `Occupation`   | Categorical    | Kode pekerjaan pengguna              | 4 (Engineer), 7 (Student)   |
|          | `Zip-code`     | String         | Kode pos pengguna                    | 53711, 10001                |
| Movies   | `MovieID`      | Integer        | Identifikasi unik film               | 1, 50, 100                  |
|          | `Title`        | String         | Judul film dan tahun rilis           | "Toy Story (1995)"          |
|          | `Genres`       | Categorical    | Genre film (dipisah dengan tanda ‘|’) | Action, Comedy, Drama       |
| Ratings  | `UserID`       | Integer        | ID pengguna yang memberi rating      | 1, 200, 5000                |
|          | `MovieID`      | Integer        | ID film yang diberi rating           | 1, 50, 100                  |
|          | `Rating`       | Integer        | Nilai rating dari 1 sampai 5         | 1, 3, 5                     |
|          | `Timestamp`    | Integer        | Waktu penilaian dalam UNIX timestamp | 874965758                   |

Untuk memahami karakteristik data, beberapa analisis dan : 
1.  **Distribusi Rating** : Distribusi rating dari seluruh data menunjukkan bahwa rating 4 dan 3 adalah yang paling banyak diberikan oleh pengguna. Rating 1 dan 5 relatif jarang.
2.  **Jumlah Rating per Pengguna** : Sebagian besar pengguna memberikan kurang dari 200 rating, dengan rata-rata sekitar 166 rating per pengguna. Ini mengindikasikan adanya variasi dalam tingkat interaksi pengguna
3.  **Genre Film Terpopuler** : Genre film yang paling banyak muncul adalah **Drama**, diikuti oleh **Comedy** dan **Thriller**. Data genre ini penting sebagai fitur utama untuk pendekatan content-based filtering
4.  **Visualisasi Genre Film** : Distribusi genre film dapat divisualisasikan menggunakan diagram batang untuk melihat frekuensi setiap genre di dataset
     
Dan berikut adalah beberapa temuan atau insight yang didapat dari hasil eksplorasi data : 
1.  **Distribusi rating menunjukkan mayoritas pengguna memberikan nilai rating antara 3 sampai 4.** : Ini mengindikasikan bahwa pengguna cenderung memberikan penilaian netral hingga positif pada film, sehingga model perlu mempertimbangkan distribusi rating yang tidak terlalu ekstrem.
2.  **Sebagian besar pengguna memberikan rating pada kurang dari 200 film, dengan rata-rata sekitar 166 rating per pengguna** : Hal ini menunjukkan adanya variasi tingkat keterlibatan pengguna, yang menimbulkan tantangan sparsity dan cold-start user, khususnya untuk pengguna dengan rating yang sedikit.
3.  **Genre film paling banyak ditemukan adalah Drama, Comedy, dan Thriller** : Keberagaman genre ini sangat membantu untuk penerapan pendekatan content-based filtering dengan fitur genre sebagai representasi konten film.
4.  **Data rating yang padat namun masih mengandung sparsity cukup tinggi** : Meskipun jumlah total rating mencapai lebih dari satu juta, sebagian besar pengguna hanya menilai sebagian kecil film. Ini menuntut penggunaan metode rekomendasi yang mampu mengatasi sparsity agar hasil rekomendasi tetap relevan.

## Data Preparation
Tahap _data preparation_ merupakan proses penting yang bertujuan untuk memastikan data dalam kondisi siap digunakan untuk pemodelan sistem rekomendasi. Tanpa data yang bersih dan terstruktur dengan baik, model rekomendasi tidak dapat bekerja secara optimal. Oleh karena itu, dilakukan serangkaian langkah transformasi dan pembersihan data secara sistematis. Berikut adalah tahapan yang dilakukan, dan disusun secara berurutan : 
1.    **Menghapus Kolom yang Tidak Relevan** : Beberapa kolom seperti `Gender`, `Age`, `Occupation`, `Zip-code`, dan `Timestamp` dihapus karena tidak memberikan kontribusi yang signifikan terhadap pembuatan sistem rekomendasi berbasis konten maupun kolaboratif
2.   **Menggabungkan Dataset** : Ketiga DataFrame digabungkan menggunakan kolom `UserID` dan `MovieID` sebagai kunci penggabungan. Hasil penggabungan ini menghasilkan satu DataFrame komprehensif yang berisi informasi pengguna, film, dan rating.
3.   **Penyederhanaan Genre** : Kolom `Genres` berisi lebih dari satu genre yang digabungkan dengan tanda "|". Untuk mempermudah analisis dan mempercepat komputasi, dilakukan penyaringan data hanya pada **3 Genre Terpopuler**
4.   **Filter Eksklusif Genre (Genre Khusus)** : Sebagai langkah lanjutan, dilakukan filter **eksklusif** untuk hanya menyertakan film-film yang memiliki **satu atau kombinasi** dari **tiga genre utama tertinggi/terpopuler**, yaitu :
        -    `Action`
        -    `Comedy`
        -    `Drama`
 
Hanya film dengan kombinasi seperti `Action`, `Comedy|Drama`, atau `Action|Comedy|Drama` yang dipertahankan. Film dengan genre diluar kategori tersebut, atau yang memiliki gabungan genre lain seperti `Action|Thriller` atau `Comedy|Romance`, **dikeluarkan dari dataset**. Filter ini bertujuan untuk : 

-    Menyederhanakan data agar lebih fokus dan tidak terlalu bervariasi
-    Mengurangi jumlah variabel unik genre yang harus diproses dalam sistem rekomendasi
-    Menghindari lonjakan kompleksitas dan konsumsi memori saat modeling

5.    **Filter Berdasarkan Aktivitas Pengguna (Top 500 User)** : Untuk mengatasi keterbatasan memori dan menghindari sparsity ekstrem, dilakukan penyaringan data pengguna. Dari keseluruhan data rating, dihitung total rating yang diberikan oleh masing-masing pengguna. Kemudia dipilih **500 pengguna teratas** dengan aktivitas rating terbanyak. Hanya data dari pengguna-pengguna ini yang dipertahankan, dan seluruh film yang pernah mereka beri rating tetap dipertahankan selama memenuhi fitur genre sebelumnya. Langkah ini bertujuan untuk :
-    Mengoptimalkan efisiensi memori dan kecepatan komputasi
-    Memastikan model dilatih pada data dari pengguna aktif
-    Menghindari noise dari pengguna pasif (dengan rating sedikit)

6.   **Menyiapkan Data untuk Collaborative Filtering** : Membuat _pivot table_ untuk memetakan UserID ke MovieID dengan isi berupa rating. Karena matriks ini merupakan input utama untuk pendekatan Collaborative Filtering berbasis matriks (user-item matrix)
7.   **Menyiapkan Data untuk Content-Based Filtering** : Mengubah genre menjadi representasi berbasis teks (TF-IDF vectorization). Karen sistem CBF membutuhkan representasi numerik dari konten film (genre) agar bisa menghitung kemiripan antar film

Beberapa alasan utama mengapa proses _data preparation_ sangat penting dalam proyek ini :
-    Menjamin **kebersihan dan konsistensi data** sebelum digunakan untuk pemodelan
-    Menyederhanakan data agar **relevan** dengan pendekatan yang digunakan
-    Menghindari **noise** dari kolom yang tidak berguna
-    Menyesuaikan format data dengan kebutuhan algoritma, misalnya **TF-IDF** untuk CBF dan **user-item matrix** untuk CF


## Modelling
Untuk menyelesaikan permasalah dalam proyek ini, dibangun dua model sistem rekomendasi dengan pendekatan yang berbeda, yaitu : 
-    **Content-Based Filtering (CBF)**
-    **Collaborative Filtering (CF)**

Kedua model dirancang untuk menyarankan film kepada pengguna berdasarkan kriteria tertentu. Pendekatan CBF merekomendasikan film mirip dengan film yang disukai pengguna, sedangkan CF memanfaatkan pola perilaku pengguna lain yang serupa untuk memberikan rekomendasi. Dataset telah difilter menggunakan genre eksklusif dan hanya menyertakan 500 pengguna dengan aktivitas rating terbanyak untuk menjaga efisiensi memori 

**Model 1 : Content-Based Filtering (CBF)** : Model CBF menghitung kemiripan antar film berdasarkan genre. Genre diformat ulang menjadi vektor TF-IDF, lalu dihitung kemiripannya menggunakan `linear_kernel`

**Input** : _Film : Amadeus (1984)"_

**Output Rekomendasi :**
-    Pleasantville (1998)
-    Raising Arizona (1987)
-    White Men Can't Jump (1992)
-    Being John Malkovich (1999)
-    Fierce Creatures (1997)

**Model 2 : Collaborative Filtering (CF)** : Model CF menggunakan pendekatan **User-Based Filtering**. Menggunakan user-item matrix dari 500 pengguna paling aktif, model menghitung kemiripan antar pengguna untuk memberikan rekomendasi berdasarkan perilaku rating pengguna serupa.

**Input** : _UserID : 26_

**Output Rekomendasi :**
-    The Shawshank Redemption (1994)
-    One Flew Over the Cuckoo's Nest (1975)
-    Monty Pythin and the Holy Grail (1974)
-    Ferris Bueller's Day Off (1986)
-    Citizen Kane (1941)

| Pendekatan                  | Kelebihan                                                                                             | Kekurangan                                                              |
|----------------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| **Content-Based Filtering** | - Tidak memerlukan data pengguna lain <br> - Cocok untuk pengguna baru <br> - Proses cepat dan ringan | - Kurang variasi rekomendasi <br> - Terbatas pada informasi item saja   |
| **Collaborative Filtering** | - Rekomendasi lebih bervariasi <br> - Menggali pola sosial pengguna                                   | - Membutuhkan banyak data rating <br> - Tidak cocok untuk pengguna baru |

# Evaluation
