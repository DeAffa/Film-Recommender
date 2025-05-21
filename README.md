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
Dataset yang digunakan dalam proyek ini bersumber dari platform <a href="https://www.kaggle.com/">Kaggle</a> dengan judul <a href="https://www.kaggle.com/datasets/lava18/google-play-store-apps">"Google Play Store Apps" </a> yang dapat diunduh atau diakses melalui tautan berikut : https://www.kaggle.com/datasets/lava18/google-play-store-apps.

Dataset ini berisi informasi mendetail mengenai aplikasi yang tersedia di Google Play Store, mulai dari nama aplikasi, kategori, rating, jumlah ulasan, ukuran file, jumlah unduhan, hingga versi Android yang dibutuhkan. File tersedia dalam format CSV dan memiliki ukuran relatif kecil, sekitar 1 MB. Secara keseluruhan, dataset ini mencakup sekitar **10.840 baris data** dengan **13 kolom fitur**. Meskipun demikian, inspeksi awal terhadap dataset menunjukkan adanya beberapa tantangan, seperti nilai kosong pada kolom penting seperti `Rating` dan `Size`, entri duplikat, format numerik yang tidak seragam (misalnya pada kolom `Installs` dan `Price`), serta keberadaan baris-baris yang tidak valid.

Berikut adalah uraian untuk seluruh fitur yang ada pada dataset "Google Play Store Apps" :
