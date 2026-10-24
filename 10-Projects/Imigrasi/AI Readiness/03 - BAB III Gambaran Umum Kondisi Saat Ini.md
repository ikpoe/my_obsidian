---
title: "BAB III GAMBARAN UMUM KONDISI SAAT INI"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB III GAMBARAN UMUM KONDISI SAAT INI

Pengelolaan data keimigrasian saat ini perlu dipotret secara utuh sebelum arah tata kelola ditetapkan. Bab ini menguraikan kondisi pengelolaan data yang terkait dengan pelaporan dan kesiapan AI, mengidentifikasi permasalahannya, serta menganalisis kesenjangannya. Uraian tersebut dimaksudkan untuk menjawab apakah proses dan tata kelola data yang berjalan sudah memadai atau masih perlu ditingkatkan terlebih dahulu sebagai syarat bagi analitik dan AI yang andal.

## 3.1 Metode dan Sumber Data Bab Ini

Uraian dalam bab ini disusun dari telaah dokumen internal Direktorat Jenderal Imigrasi, berupa kajian dan laporan teknis yang relevan serta inventori sistem dan basis data, yang dilengkapi klarifikasi dari pemilik dokumen, termasuk mengenai peran Pusat Data Keimigrasian (PUSDAKIM). Hasil telaah tersebut kemudian dipetakan terhadap kerangka regulasi yang berlaku.

## 3.2 Gambaran Umum Kelembagaan

Secara historis, pengelolaan data keimigrasian berorientasi pada penyusunan laporan, pemantauan dan evaluasi, serta pemenuhan permintaan data. Dalam pembagian tugasnya, DITTIK mengelola platform dan operasional sistem, sedangkan fungsi pengelolaan dan pemanfaatan data menjadi titik berat pemakaian data. Arah penguatan kemampuan platform AI juga telah diusulkan, dengan ketergantungan pada data terkurasi dan tata kelola yang memadai.

Selain itu, terdapat dua tantangan kelembagaan. Pengadaan dan pengembangan sistem cenderung berjalan per modul tanpa orkestrasi integrasi yang menyeluruh, sementara personel internal untuk integrasi dan alih pengetahuan masih terbatas. Kedua tantangan tersebut berpengaruh langsung terhadap dimensi Orang dan Proses dalam kerangka penilaian kematangan.

## 3.3 Gambaran Umum Sistem Informasi dan Basis Data

SIMKIM mengintegrasikan sekitar 40 jenis aplikasi, perangkat operasional, serta interkoneksi dengan kementerian/lembaga (K/L) dan badan usaha. Banyaknya interkoneksi tersebut memperluas risiko pada aspek akses, klasifikasi, audit, dan PDP. Dari sisi infrastruktur, layanan publik sempat dioperasikan pada GCP dan AWS sekitar kejadian PDN Juni 2024. Seluruh layanan tersebut kemudian dipindahkan ke AWS, yang kini menjadi pusat data utama.

Sementara itu, PUSDAKIM saat ini berfungsi sebagai lokasi *disaster recovery* (DR). Direktorat Jenderal Imigrasi memiliki rencana jangka panjang untuk kembali memanfaatkan PUSDAKIM dengan memindahkan aplikasi agar berjalan di sana, tetapi proses tersebut masih memerlukan waktu yang panjang.

Kondisi ini membawa dua implikasi bagi tata kelola data. Kebijakan klasifikasi, akses, dan retensi harus berlaku sama, baik di lokasi utama maupun di lokasi DR. Apabila kelak aplikasi dipindahkan ke PUSDAKIM, perpindahan tersebut perlu disertai penerapan kendali tata kelola yang sama secara bertahap sehingga tidak berhenti sebagai perpindahan infrastruktur semata.

## 3.4 Gambaran Umum Pengelolaan Data Analitik

Sistem Big Data Imigrasi terdiri atas platform sinkronisasi data dari banyak satuan kerja (DSP), proses migrasi dan konsolidasi menuju penyimpanan analitik, orkestrasi MDM dan ETL, *worker*, komponen visualisasi, serta penyimpanan analitik berbasis PostgreSQL berskala besar. Skala pemrosesannya sudah besar, sebagaimana tercermin dari koneksi ke banyak basis data sumber serta banyaknya proses ETL dan *job* harian. Sistem Big Data kini berada di AWS bersama aplikasi layanan, dan pengoperasiannya melibatkan mitra *managed service*.

Secara konsep, pemisahan beban transaksi dari beban analitik sudah dijalankan. Beban transaksi harian, seperti permohonan paspor, dilayani oleh sistem *online transaction processing* (OLTP). Namun, dari sudut pandang tata kelola data modern, katalog, keamanan berbasis kebijakan, pemantauan kualitas, dan *lineage* belum sebanding dengan volume ETL. Perbandingan data antarsiklus untuk mendeteksi anomali dan menelusuri kebenaran angka juga masih menjadi tantangan. Gambaran umum arsitektur data saat ini secara konseptual ditunjukkan pada Gambar 1.

![[Gambar 1 - Arsitektur Data Saat Ini.png]]

*Gambar 1. Gambaran Umum Arsitektur Data Saat Ini (Konseptual). SISTIK dikelola oleh DITTIK, aplikasi layanan dan Big Data berada di AWS, dan PUSDAKIM berfungsi sebagai lokasi DR. Sumber: hasil olahan kajian.*

## 3.5 Gambaran Umum Pemanfaatan Data dan Pelaporan

Data keimigrasian terutama dimanfaatkan untuk memantau kinerja layanan melalui *dashboard* pelaporan dan untuk memenuhi permintaan data. Portal permintaan data eksternal cenderung kuat dalam memvalidasi permohonan, tetapi penyiapan format datanya sering kali masih dilakukan secara manual. Selain itu, hasil olahan data belum selalu kembali ke proses operasional, sebagaimana terlihat pada contoh *overstay* dan proses keberangkatan. Kondisi tersebut menunjukkan bahwa persoalan utamanya tidak terletak pada satu produk *dashboard*, melainkan pada pengelolaan seluruh rantai data yang menyuplainya.

## 3.6 Gambaran Umum Sumber Daya Manusia

Kapasitas SDM pengelola data saat ini merupakan gabungan personel internal dan mitra *managed service*. Peran teknis pengolahan data lebih menonjol dibandingkan peran pemilik data dan *steward* yang baku di sepanjang rantai pelaporan. Ketimpangan tersebut berisiko menjadikan pengetahuan tentang isi data dan definisi angka terpusat pada pihak teknis atau mitra. Atas pertimbangan tersebut, rekomendasi peran dalam kajian ini dirumuskan secara ringkas agar tidak tumpang tindih dengan desain organisasi platform AI.

## 3.7 Identifikasi Permasalahan

Berdasarkan gambaran kondisi di atas, permasalahan utama tata kelola data di lingkungan Direktorat Jenderal Imigrasi berakar pada orientasi pengelolaan data yang masih bertumpu pada keluaran laporan tanpa diimbangi tata kelola sepanjang rantai data. Katalog, *lineage*, dan pemantauan kualitas belum sebanding dengan volume data masuk dan ETL, sementara kebijakan klasifikasi informasi yang berlaku tunggal di tingkat direktorat belum teridentifikasi. Tanpa metrik yang tersertifikasi, angka resmi sulit ditelusuri dan dipertanggungjawabkan. Kondisi tersebut juga membuka risiko urutan terbalik, yaitu platform dan AI berjalan lebih dahulu daripada fondasi data.

Di samping itu, infrastruktur yang tersebar di lebih dari satu lokasi, yaitu pusat data utama dan lokasi DR, serta banyaknya interkoneksi eksternal memperbesar kebutuhan akan *guardrail* PDP dan keamanan serta kejelasan kepemilikan data. Umpan balik ke operasional juga masih lemah karena hasil analitik belum selalu kembali ke proses layanan, sebagaimana terlihat pada kasus *overstay*. Hubungan antara akar masalah, masalah inti, dan akibatnya dipetakan pada Gambar 2.

![[Gambar 2 - Peta Permasalahan.png]]

*Gambar 2. Peta Permasalahan Tata Kelola Data. Sumber: hasil olahan kajian.*

## 3.8 Analisis Kesenjangan

Kesenjangan antara kondisi saat ini dan kondisi yang dituju dirangkum pada tabel berikut. Setiap kesenjangan diberi kode G1 sampai dengan G6.

| Kode | Kesenjangan |
|----|----|
| G1 | Pemilik data dan *steward* sepanjang rantai pelaporan perlu ditetapkan secara baku. |
| G2 | Klasifikasi informasi perlu dijalankan sebagai dasar pemberian akses dan pemakaian AI. |
| G3 | Katalog, *lineage*, dan pemantauan belum sebanding dengan volume pemrosesan. |
| G4 | Definisi dan proses rilis metrik untuk angka resmi perlu ditetapkan secara formal. |
| G5 | *Guardrail AI/BI perlu dikaitkan dengan kelas data dan proses rilis angka.* |
| G6 | Arah platform perlu dipisahkan dari rincian merek layanan, dan kebijakan perlu berlaku sama di lokasi utama maupun lokasi DR. |

## 3.9 Asumsi dan Kebutuhan Validasi

Uraian kondisi saat ini bertumpu pada sejumlah asumsi yang masih memerlukan validasi. Kebijakan klasifikasi data formal yang berlaku tunggal di tingkat direktorat diasumsikan belum tersedia. Daftar sistem yang tersedia diperlakukan sebagai gambaran kondisi saat ini dan bukan sebagai kamus data yang lengkap. Angka kapasitas, yaitu ukuran penyimpanan, jumlah koneksi, jumlah proses ETL, dan jumlah *job* harian, tidak digunakan dalam uraian karena belum dapat divalidasi, dan pengukuran ulangnya direkomendasikan sebagai tindak lanjut. Uraian yang belum terkonfirmasi perlu diverifikasi melalui wawancara dengan unit terkait, yang dapat digabungkan dengan penilaian kematangan pada *Checkpoint* 0.
