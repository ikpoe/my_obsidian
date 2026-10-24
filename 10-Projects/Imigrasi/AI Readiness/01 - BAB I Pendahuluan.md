---
title: "BAB I PENDAHULUAN"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB I PENDAHULUAN

Bab ini menguraikan latar belakang, maksud dan tujuan, sasaran, ruang lingkup, landasan hukum, metodologi penyusunan, serta sistematika penulisan kajian.

## 1.1 Latar Belakang

Penataan kelembagaan melalui Peraturan Presiden Nomor 157 Tahun 2024 tentang Kementerian Imigrasi dan Pemasyarakatan dan Peraturan Menteri Imigrasi dan Pemasyarakatan Nomor 1 Tahun 2024 tentang Organisasi dan Tata Kerja menuntut Direktorat Jenderal Imigrasi untuk menyelenggarakan tata kelola data yang andal dan akuntabel. Arah kebijakan tersebut sejalan dengan agenda Sistem Pemerintahan Berbasis Elektronik (SPBE) dan Satu Data Indonesia yang mendorong pemanfaatan data secara terpadu di lingkungan pemerintahan. Berkenaan dengan hal di atas, kesiapan data menjadi prasyarat bagi setiap upaya pemanfaatan analitik maupun kecerdasan buatan di lingkungan keimigrasian.

Fungsi keimigrasian meliputi pelayanan, penegakan hukum, keamanan negara, dan fasilitasi pembangunan kesejahteraan masyarakat. Seluruh fungsi tersebut dijalankan melalui Sistem Informasi Manajemen Keimigrasian (SIMKIM) yang mengintegrasikan sekitar 40 jenis aplikasi layanan dan pendukung. Keterpaduan aplikasi dalam jumlah sebesar itu menghasilkan data dengan volume yang besar dan beragam. Kondisi tersebut menjadikan SIMKIM sebagai sumber utama bagi pelaporan, pengawasan, dan perumusan kebijakan di lingkungan Direktorat Jenderal Imigrasi.

Penyelenggaraan sistem tersebut turut dipengaruhi oleh kejadian pada Pusat Data Nasional (PDN). Sekitar kejadian PDN Juni 2024, layanan publik di lingkungan *production* untuk beberapa waktu dioperasikan pada dua layanan awan publik, yakni Google Cloud Platform (GCP) dan Amazon Web Services (AWS). Seluruh layanan tersebut kemudian dipindahkan ke AWS sehingga AWS kini menjadi pusat data utama.

Sistem pengelolaan data analitik (Big Data Imigrasi) juga telah berada di AWS, dan GCP tidak digunakan lagi. Basis data analitik tersebut berskala besar, terhubung ke banyak basis data sumber, serta menjalankan banyak proses *extract, transform, load* (ETL) dan *job* harian.

Namun, skala teknis tersebut belum diikuti oleh pemanfaatan data yang sebanding. Data masih lebih banyak digunakan untuk penyusunan laporan dan pemenuhan permintaan data, dan hasil olahannya belum selalu kembali memperkuat kegiatan operasional. Sebagai contoh, temuan potensi *overstay* belum dimanfaatkan secara langsung pada proses keberangkatan.

Di sisi lain, minat untuk memanfaatkan kecerdasan buatan (AI) dan *business intelligence* (BI) dalam menyusun dan menjelajahi laporan mulai tumbuh, antara lain melalui usulan penguatan kemampuan platform AI. Pemanfaatan AI dan *dashboard* yang lebih cerdas justru membawa risiko baru apabila rantai datanya tidak terkelola dari ujung ke ujung. Rantai tersebut mencakup sistem sumber, pengendalian kualitas dan akses, hingga pemakaian data untuk laporan dan, bila diperlukan, untuk melatih atau menjalankan model AI.

Sebagian fondasi teknis, termasuk *data warehouse* dan platform analitik, telah dibangun. Namun, masih terdapat celah tata kelola, yaitu katalog data, *lineage*, pemantauan kualitas, klasifikasi informasi, kepemilikan data, dan pengendalian akses yang belum semapan volume pemrosesannya. Celah tersebut berpotensi melemahkan kepercayaan terhadap angka laporan dan memperbesar risiko ketika AI mulai dimanfaatkan secara luas. Oleh karena itu, perlu dipahami lebih lanjut kondisi tata kelola data yang ada sebelum investasi analitik dan AI diperluas.

Berdasarkan hal tersebut, Direktorat Jenderal Imigrasi memandang perlu adanya kajian tata kelola data untuk kesiapan kecerdasan buatan. Kajian ini menitikberatkan kebijakan dan desain tata kelola sepanjang rantai data pelaporan. Arah platform diuraikan secara konseptual dan netral terhadap merek layanan. Pelaksanaan rekomendasi diatur melalui *checkpoint* bertahap yang menentukan syarat sebelum pelaksanaan berlanjut ke tahap berikutnya.

## 1.2 Maksud dan Tujuan

Maksud kajian ini adalah memberikan arah kebijakan, kerangka desain, dan rekomendasi tindak lanjut tata kelola data di lingkungan keimigrasian agar data dapat dipercaya, mematuhi ketentuan, dan siap mendukung analitik serta AI secara bertanggung jawab.

Adapun tujuan kajian ini adalah menyusun rekomendasi kebijakan tata kelola data ujung ke ujung bagi rantai data yang menyuplai pelaporan dan analitik. Kajian ini juga mengusulkan kerangka penilaian kematangan yang mencakup dimensi orang, proses, data, platform, dan kesiapan AI/BI serta selaras dengan tingkat kapabilitas SPBE, dengan penilaian yang dijalankan pada *Checkpoint* 0. Selain itu, kajian ini menetapkan prinsip kondisi yang dituju dan arah kemampuan platform secara konseptual, termasuk pemisahan beban operasional dan analitik. Tujuan berikutnya adalah menetapkan kerangka klasifikasi informasi dan *guardrail* pemakaian AI/BI untuk angka resmi dan data sensitif. Seluruh tujuan tersebut dirangkai dalam *checkpoint* pelaksanaan yang dapat dijalankan tanpa terikat pada jadwal kalender yang kaku.

## 1.3 Sasaran

Bagian ini menjelaskan sasaran yang ingin dicapai melalui kajian ini. Sasaran tersebut adalah sebagai berikut.

1.  Terdapat kejelasan peran minimal tata kelola data pada domain pelaporan prioritas.

2.  Tersusun skema klasifikasi informasi dan register objek data prioritas sepanjang rantai pelaporan.

3.  Rantai data, mulai dari sumber, pengolahan, dan metrik hingga pemakaian laporan, terpetakan beserta celah kualitas dan *lineage*-nya.

4.  Investasi platform data dan AI tidak mendahului fondasi tata kelola.

5.  Risiko keputusan berbasis angka yang tidak dapat dipertanggungjawabkan atau melanggar pelindungan data pribadi berkurang.

## 1.4 Ruang Lingkup

Ruang lingkup kajian ini dibatasi pada tiga aspek, yaitu lingkup substansi, lingkup organisasi, dan batasan. Ketiga aspek tersebut diuraikan sebagai berikut.

**1. Lingkup substansi.** Kajian ini mencakup tata kelola data sepanjang rantai pelaporan dan analitik yang meliputi kepemilikan, klasifikasi, akses, kualitas, metadata dan *lineage*, serta prinsip pemakaian data untuk *dashboard*, laporan, dan AI/BI. Arsitektur dibahas pada tingkat konsep dan spesifikasi kemampuan, bukan sebagai resep konfigurasi produk awan tertentu. *Dashboard* pelaporan hanya diposisikan sebagai salah satu titik pemakaian data karena objek tata kelolanya adalah seluruh rantai data. Desain tim platform AI berada di luar cakupan sehingga pembahasannya dibatasi pada aspek tata kelola data yang menopangnya.

**2. Lingkup organisasi.** Kajian ini mencakup Direktorat Jenderal Imigrasi, khususnya DITTIK serta fungsi pengelolaan dan pemanfaatan data dan pelaporan. Perencanaan AI untuk Direktorat Jenderal Pemasyarakatan berada di luar cakupan. Sinergi lintas direktorat jenderal mengikuti prinsip Satu Data tingkat kementerian.

**3. Batasan.** Pembahasan tidak mencakup implementasi rinci layanan awan tertentu, pembangunan model AI yang lengkap untuk semua kebutuhan, maupun matriks pembagian peran RACI (*Responsible, Accountable, Consulted, Informed*) yang sangat rinci. Terkait prosedur operasional standar (SOP), kajian ini hanya menyediakan daftar SOP yang perlu disusun dan tidak memuat isi SOP tersebut, yang selanjutnya disusun oleh unit terkait. Penetapan domain pilot juga berada di luar cakupan karena merupakan kewenangan pimpinan; kajian ini hanya menyediakan kriteria pemilihannya. Demikian pula, penilaian kematangan tidak dilaksanakan dalam kajian ini, melainkan pada *Checkpoint* 0 dengan kerangka yang diusulkan. Uraian kapasitas data disajikan secara kualitatif karena angka kapasitas yang tersedia belum dapat divalidasi. Rincian implementasi pada platform awan tertentu dapat menjadi materi kajian lanjutan.

## 1.5 Landasan Hukum

Penyelenggaraan tata kelola data di lingkungan Direktorat Jenderal Imigrasi berpijak pada peraturan perundang-undangan yang mengatur keimigrasian, pelayanan publik, informasi dan transaksi elektronik, keterbukaan informasi publik, pelindungan data pribadi, SPBE dan transformasi digital, Satu Data Indonesia, kelembagaan kementerian, serta keamanan informasi. Peraturan tersebut secara bersama-sama menentukan kewenangan dan kewajiban Direktorat Jenderal Imigrasi dalam mengelola, melindungi, dan membagi pakai data. Peraturan yang sama juga menetapkan batas pemanfaatan data untuk analitik dan AI sehingga menjadi landasan bagi seluruh rekomendasi dalam kajian ini. Adapun peraturan yang menjadi landasan hukum kajian ini adalah sebagai berikut.

1.  Undang-Undang Nomor 6 Tahun 2011 tentang Keimigrasian.

2.  Undang-Undang Nomor 25 Tahun 2009 tentang Pelayanan Publik.

3.  Undang-Undang Nomor 11 Tahun 2008 tentang Informasi dan Transaksi Elektronik beserta perubahannya.

4.  Undang-Undang Nomor 14 Tahun 2008 tentang Keterbukaan Informasi Publik.

5.  Undang-Undang Nomor 27 Tahun 2022 tentang Pelindungan Data Pribadi.

6.  Peraturan Presiden Nomor 95 Tahun 2018 tentang Sistem Pemerintahan Berbasis Elektronik.

7.  Peraturan Presiden Nomor 39 Tahun 2019 tentang Satu Data Indonesia.

8.  Peraturan Presiden Nomor 82 Tahun 2023 tentang Percepatan Transformasi Digital dan Keterpaduan Layanan Digital Nasional.

9.  Peraturan Presiden Nomor 157 Tahun 2024 tentang Kementerian Imigrasi dan Pemasyarakatan.

10. Peraturan Menteri Imigrasi dan Pemasyarakatan Nomor 1 Tahun 2024 tentang Organisasi dan Tata Kerja.

11. Peraturan Badan Siber dan Sandi Negara Nomor 8 Tahun 2020 tentang Sistem Pengamanan dalam Penyelenggaraan Sistem Elektronik.

12. Peraturan Badan Siber dan Sandi Negara Nomor 4 Tahun 2021 tentang Pedoman Manajemen Keamanan Informasi SPBE dan Standar Teknis serta Prosedur Keamanan SPBE, termasuk penetapan klasifikasi informasi.

13. Peraturan Menteri Pendayagunaan Aparatur Negara dan Reformasi Birokrasi Nomor 59 Tahun 2020 tentang Pemantauan dan Evaluasi Sistem Pemerintahan Berbasis Elektronik.

## 1.6 Metodologi Penyusunan

Kajian ini disusun melalui analisis dokumen internal, berupa kajian dan laporan teknis yang relevan serta inventori sistem, yang dilengkapi klarifikasi dari pemilik dokumen. Temuan dari dokumen tersebut kemudian dipetakan terhadap ketentuan SPBE, Satu Data, pelindungan data pribadi (PDP), dan keamanan informasi SPBE.

Untuk menilai kesiapan organisasi, kajian ini mengusulkan kerangka penilaian kematangan pada lima dimensi, yaitu Orang, Proses, Data, Platform, dan Kesiapan AI/BI, dengan tata kelola sebagai pengikat kelima dimensi tersebut. Pendekatan ini mengikuti semangat kerangka P3GC (*People, Process, Platform, Governance, Change*), tetapi tidak menjadikan pembentukan tim platform AI sebagai pusat pembahasan. Penilaian dengan kerangka tersebut dijalankan pada *Checkpoint* 0. Badan naskah memuat arah kebijakan, *checkpoint*, dan rekomendasi kebijakan bagi pimpinan, dan rincian operasional yang bersifat teknis dimuat dalam lampiran.

## 1.7 Sistematika Penulisan

Sistematika penulisan kajian ini terdiri atas Bab I Pendahuluan; Bab II Landasan Regulasi, Arah Kebijakan, dan Literatur; Bab III Gambaran Umum Kondisi Saat Ini; Bab IV Tujuan, Sasaran, dan Prinsip Tata Kelola Data untuk Kesiapan AI; Bab V Spesifikasi Arah Arsitektur Data dan Informasi; Bab VI Rekomendasi Tata Kelola Data; Bab VII Pengembangan Kapasitas dan Budaya Data; Bab VIII Pemanfaatan Analitik dan AI, Penilaian Kematangan, *Checkpoint* Implementasi, dan Manajemen Risiko; Bab IX Penutup; Daftar Pustaka; serta Lampiran.
