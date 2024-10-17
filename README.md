
---
# **WELCOME TO MY PORFOTOLIO**

Bagian ini berisi portofolio yang menampilkan pengalaman kerja saya serta partisipasi dalam bootcamp di Purwadhika, di mana saya mengembangkan keterampilan dan pengetahuan di bidang data.


## 1. Optimasi Data Indonesia - Data Science
### Maret 2024 - Sekarang
**1. Football Player Statistic**
   
<p align="justify">
Pada project ini saya mengemban tanggung jawab untuk menyiapkan use case terkait statistik pemain bola kepada calon klien dari salah satu klub sepak bola terbesar di Indonesia. Data diambil dari Website <a href="https://www.sofascore.com">Sofascore</a> menggunakan API yang tersedia. Saya mencoba mencari API yang tersedia secara gratis selain Sofascore namun sangat sulit karena rata-rata penyedia layanan API untuk statistik pemain bola adalah langganan yang berbayar. Sayangnya Sofascore hanya menyediakan statistik pemain berdasarkan overall-nya saja sehingga cukup sulit menentukan perkembangan pemain dari tiap match yang dijalankan. Data-data pemain yang diambil adalah pemain dari 5 liga top Eropa (Liga Inggris, Perancis, Italia, Spanyol, dan Jerman) dan BRI Liga 1 Indonesia.
</p>

![Alt Text](/pic/football_flow_1.jpg)
Gambar 1.1
<p align="justify">
Proses ekstrak data dari web tersebut dijalankan menggunakan <strong>Apache Airflow</strong> sebagai data orchestration tool yang dijadwalkan menarik data-data tersebut lewat API seminggu sekali dengan melewatkan jadwal pertandingan antar negara dari FIFA. Data yang diambil dimasukkan ke <strong>OpenSearch</strong> untuk divisualisasikan.
</p>

Hasil visualisasi dapat diatur menggunakan timestamp yang diset satu minggu yang lalu sehingga data yang lama tidak akan ikut tervisualisasi. Berikut dashboard yang dibuat:

![Alt Text](/pic/football_1.jpg)
Gambar 1.2
![Alt Text](/pic/football_2.jpg)
Gambar 1.3
![Alt Text](/pic/football_3.jpg)
Gambar 1.4

Ketiga gambar dashboard di atas bersifat interaktif sehingga dapat dilakukan pemfilteran terhadap nama klub atau nama pemain.

**2. Aplikasi Kementerian**

<p align="justify">
Pada project ini saya berkesempatan menjadi PIC untuk mengalirkan data-data sebuah kementerian dari database mereka (PostgreSQL) menjadi satu aplikasi khusus. Data-data tersebut berisi anggaran daerah yang secara internal perlu disebarluaskan ke pemerintah-pemerintah daerah menggunakan aplikasi yang akan dibuat. Perusahaan saya dipilih menjadi penanggungjawab database aplikasi tersebut dengan membawa produk kami yaitu <a href="https://onyx.id/">Onyx Big Data Platform</a>. Onyx banyak memanfaatkan aplikasi open source yang pada projek ini aplikasi yang digunakan adalah <strong>Apache Hadoop</strong>, <strong>Apache Airflow</strong>, <strong>Apache Hive</strong>, <strong>Apache Zeppelin</strong>, <strong>Apache Spark</strong>, dan <strong>OpenSearch</strong>.
</p>

![Alt Text](/pic/application_flow.jpg)

Gambar 2.1

<p align="justify">
Apache Airflow bertugas sebagai orchestration tool untuk melakukan penjadwalan mengalirkan data dari database kementerian ke <strong>Apache Hive</strong> dan <strong>OpenSearch</strong>. Pemrograman menggunakan <strong>bahasa python</strong> dengan memanfaatkan <strong>Apache Spark</strong> dalam melakukan transformasi data yang berat. Hasil pemrograman di Apache Airflow berbentuk <strong>DAG (Directed Acyclic Graph)</strong> yang jumlahnya ratusan DAG. Sebelum proses production, banyak simulasi yang dilakukan di notebook terutama menggunakan <strong>Apache Zeppelin</strong> sebelum akhirnya menjadi DAG yang siap digunakan. Berikut adalah alur dari DAG yang diciptakan.
</p>

![Alt Text](/pic/dag.png)
Gambar 2.2

<p align="justify">
Data yang masuk ke Apache Hive tidak dilakukan pengolahan data karena data yang masuk harus sama dengan sumber data sebagai data yang asli. Sedangkan pengolahan data dilakukan jika data tersebut masuk sebagai Index OpenSearch meliputi <strong>data cleansing</strong> dan <strong> data transforming</strong>.
</p>

Data cleansing dan data transforming adalah dua tahap penting dalam proses pengolahan data, tetapi keduanya memiliki fokus dan tujuan yang berbeda.

Data cleansing, atau pembersihan data, adalah proses yang bertujuan untuk mengidentifikasi dan mengoreksi kesalahan dalam data. Proses ini mencakup berbagai aktivitas, seperti:
1. **Identifikasi Kesalahan**: Memeriksa data untuk menemukan anomali, seperti duplikasi, nilai yang hilang, atau ketidakcocokan format. Misalnya, jika dalam suatu dataset terdapat nilai kosong atau entri yang salah ketik, langkah awal adalah mengidentifikasi masalah ini.
2. **Penghapusan Duplikasi**: Menghilangkan entri yang sama dalam dataset untuk memastikan bahwa setiap record unik.
3. **Koreksi Data**: Memperbaiki kesalahan dalam data, seperti mengubah format tanggal yang tidak konsisten atau memperbaiki kesalahan ejaan. Ini penting untuk memastikan bahwa data dapat digunakan untuk analisis yang akurat.
4. **Validasi Data**: Memastikan bahwa data yang dikumpulkan memenuhi kriteria tertentu, seperti rentang nilai yang wajar atau format yang benar. Contohnya seperti melibatkan pemeriksaan apakah semua alamat email dalam format yang benar.

Data transforming, atau transformasi data, adalah proses yang bertujuan untuk mengubah data menjadi format yang lebih sesuai untuk analisis atau pemrosesan lebih lanjut. Proses ini mencakup berbagai aktivitas, seperti:
1. **Normalisasi**: Mengubah skala data sehingga dapat dibandingkan secara konsisten. Misalnya, jika kita memiliki data anggaran dalam mata uang yang berbeda, kita mungkin perlu mengonversi semuanya ke dalam satu mata uang.
2. **Agregasi**: Menggabungkan data dari beberapa sumber atau tingkat granularitas yang berbeda untuk memberikan pandangan yang lebih holistik. Contohnya, menjumlahkan total anggaran per kegiatan atau sub-kegiatan.
3. **Pembuatan Fitur**: Menghasilkan variabel baru dari data yang ada untuk meningkatkan model analisis atau prediksi seperti flagging daerah miskin ekstrim.
4. **Perubahan Struktur Data**: Mengubah format data, seperti memindahkan dari format tabel ke format JSON (atau sebaliknya), atau mengubah tipe data (misalnya, dari string ke integer) agar sesuai dengan kebutuhan aplikasi SIPD Hub.

Hasil aplikasi:

![Alt Text](/pic/aplikasi_1.jpg)
Gambar 2.3
![Alt Text](/pic/aplikasi_2.jpg)
Gambar 2.4

## 2. Telkomsel Orbit - Data Analyst
### Juni 2023 - Desember 2023
<p align="justify">
Telkomsel adalah tempat pertama kali saya bekerja secara profesional, maka dari itu saya lebih banyak membantu user saya dalam mengerjakan projek-projek Telkomsel Orbit. Sebagai data analis, saya lebih banyak menyusun reporting dashboard secara mingguan dan bulanan. Di sini saya memanfaatkan <strong>Microsoft Excel (terutama fitur pivot)</strong>, <strong>Microsoft Power Point</strong>, <strong>Tableau</strong>, <strong>Jupyter Notebook</strong>, dan <strong>SQL</strong>. Selain membantu user saya, saya mengemban tanggung jawab sebagai satu-satunya orang untuk memberikan kebutuhan data dari rekan-rekan satu divisi telkomsel orbit menggunakan HiveQL. Kebutuhan data dari rekan-rekan bervariasi. Mulai dari raw data sampai hasil analisa yang berbentuk keputusan terutama dalam menjalankan campaign terkait produk telkomsel orbit.
</p>

Berikut adalah beberapa dashboard yang menjadi tanggung jawab saya:

![Alt Text](/pic/telkomsel_1.jpg)
**Informasi angka dan kalimat dari gambar di atas tidak valid demi menjaga kerahasiaan Telkomsel.**

![Alt Text](/pic/telkomsel_2.jpg)
**Informasi angka dan kalimat dari gambar di atas tidak valid demi menjaga kerahasiaan Telkomsel.**

---

## 3. Purwadhika Digital Technology School - Bootcamp Data Science
### Mei 2022 - Oktober 2022
<p align="justify">
Sebagai lulusan elektro yang menempuh bidang arus lemah, saya memiliki basik konsep yang kuat terhadap pemrograman. Untuk memoles pemrograman saya, saya tertarik untuk berkarir di bidang data yang terus memanfaatkan ilmu pemrograman di dalamnya sehingga saya memilih purwadhika dengan mengambil kelas Data Science and Machine Learning. Di sini saya belajar tentang <strong>pemrograman dasar</strong>, <strong>statistika</strong>, <strong>dan Machine learning</strong>. Beberapa project dari dan setelah lulus dari purwadhika dapat diakses di sini:
</p>

- [Crime in Boston](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Crime%20in%20Boston)
- [Hotel Book Cancellation](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Hotel%20Cancellation)
- [Olist Store](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Olist%20Store)
- [Saudi Arabia Used Car](https://github.com/MuhammadMukhlis220/Porfotolio_Project/tree/main/Saudi%20Arabia%20Used%20Car)

---
Connect me in [LinkedIn](www.linkedin.com/in/mmukhlis10) or touch my social media in [Twitter](https://twitter.com/bobyjhow).

Thank You
