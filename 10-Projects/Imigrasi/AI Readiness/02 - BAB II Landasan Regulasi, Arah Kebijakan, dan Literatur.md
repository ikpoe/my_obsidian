---
title: "BAB II LANDASAN REGULASI, ARAH KEBIJAKAN, DAN LITERATUR"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB II LANDASAN REGULASI, ARAH KEBIJAKAN, DAN LITERATUR

Penyelenggaraan tata kelola data yang siap AI terikat pada sejumlah peraturan dan rujukan yang berdampak langsung terhadap tiga hal, yaitu pengelolaan data sepanjang rantai pelaporan dan analitik; klasifikasi, akses, dan pelindungan data; serta batas pemakaian AI/BI di atas data yang dapat dipertanggungjawabkan. Peraturan dan rujukan tersebut diuraikan menurut tema, bukan pasal demi pasal, agar keterkaitannya dengan kebutuhan tata kelola data lebih mudah dipahami. Setiap tema diakhiri dengan implikasi bagi tata kelola data yang menjadi dasar bagi rekomendasi kajian ini.

## 2.1 Arah Transformasi Digital dan SPBE

Peraturan Presiden (Perpres) Nomor 82 Tahun 2023 menekankan percepatan transformasi digital dan keterpaduan layanan digital nasional. Arah kebijakan ini memperkuat alasan bagi instansi pemerintah untuk berinvestasi pada data yang terintegrasi, dapat dibagi pakai secara terkendali, dan siap mendukung pengambilan keputusan berbasis data, termasuk melalui analitik dan AI.

Sementara itu, Perpres Nomor 95 Tahun 2018 tentang SPBE menjadi kerangka induk penyelenggaraan teknologi informasi dan komunikasi (TIK) pemerintahan yang terintegrasi, efisien, transparan, dan akuntabel. SPBE mendorong pemanfaatan teknologi baru seperti analitik dan AI dengan syarat arsitektur, tata kelola, dan manajemennya tertib. Pemantauan dan evaluasi SPBE, termasuk Indeks SPBE, mendorong perbaikan yang berkelanjutan pada setiap instansi. Meskipun tidak dimaksudkan sebagai alat penilaian Indeks SPBE, rekomendasi dalam kajian ini disusun agar kontribusi tata kelola data mudah ditelusuri pada aspek kebijakan manajemen data, keamanan informasi, integrasi layanan, dan kualitas data.

**Implikasi bagi tata kelola data. Keterpaduan layanan** menuntut interoperabilitas dan standar data, bukan sekadar pengadaan aplikasi yang berdiri sendiri. Arsitektur data dan aplikasi juga tidak dapat dibiarkan tumbuh sebagai silo tanpa koordinasi. Keamanan dan manajemen risiko pun perlu melekat sejak awal dan bukan ditambahkan pada akhir proyek. Percepatan digital tanpa tata kelola justru memperbesar utang teknis dan risiko data, mengingat AI memanfaatkan fondasi data dan tidak dapat menggantikannya. Untuk itu, *checkpoint* pelaksanaan yang diusulkan dirancang agar dapat dipetakan ke indikator SPBE pada tahap berikutnya.

## 2.2 Kerangka Kebijakan Data: Satu Data Indonesia, Standar, dan Metadata

Perpres Nomor 39 Tahun 2019 tentang Satu Data Indonesia mewajibkan data pemerintah memenuhi standar data, memiliki metadata, memenuhi kaidah interoperabilitas, serta menggunakan kode referensi dan/atau data induk. Kelembagaan Satu Data juga membedakan peran pembina data, walidata, dan produsen data sehingga setiap data memiliki penanggung jawab yang jelas.

**Implikasi bagi tata kelola data**. Ketentuan tersebut menuntut agar angka resmi dan indikator laporan di lingkungan Direktorat Jenderal Imigrasi memiliki definisi, metadata, dan penanggung jawab yang jelas, sejalan dengan peran walidata dan produsen data tanpa menggantikan Forum Satu Data tingkat kementerian. Interoperabilitas, baik di dalam SIMKIM maupun antarinstansi, membutuhkan katalog dan standar data, bukan hanya koneksi teknis. Data induk dan data referensi menjadi fondasi agar pemanfaatan AI/BI tidak menggunakan definisi yang saling bertentangan. Atas dasar itu, rekomendasi tata kelola dalam kajian ini disusun selaras dengan Satu Data, khususnya dalam hal register, metadata, dan kepemilikan data.

## 2.3 Pelindungan Data Pribadi dan Keamanan Informasi

Undang-Undang (UU) Nomor 27 Tahun 2022 tentang Pelindungan Data Pribadi mengatur asas, hak subjek data, kewajiban pengendali dan prosesor data, serta syarat pemrosesan data pribadi. Undang-undang ini juga mengatur data pribadi yang bersifat spesifik, misalnya data biometrik, yang sangat relevan bagi layanan keimigrasian. Dari sisi keamanan, aspek kerahasiaan, integritas, dan ketersediaan dalam SPBE diperkuat oleh Peraturan Badan Siber dan Sandi Negara (BSSN) Nomor 8 Tahun 2020 dan Peraturan BSSN Nomor 4 Tahun 2021. Peraturan BSSN Nomor 4 Tahun 2021 menekankan penetapan klasifikasi informasi sebagai bagian dari pemenuhan aspek kerahasiaan.

**Implikasi bagi tata kelola data**. Berdasarkan ketentuan tersebut, pemrosesan data untuk laporan, analitik, pelatihan model, maupun pemakaian AI membutuhkan dasar pemrosesan yang sah dan tujuan yang jelas. Prinsip minimisasi data, pembatasan tujuan, keamanan, dan akuntabilitas perlu dirancang sejak awal. Klasifikasi informasi juga harus dijalankan dan tidak berhenti sebagai dokumen sehingga klasifikasi melekat pada objek data, hak akses, kanal pemakaian seperti *dashboard* dan AI, serta audit. Berbagi data dengan mitra *managed service* dan penyedia layanan awan pun memerlukan kejelasan peran pengendali dan prosesor serta jaminan keamanan. Usulan skema klasifikasi menjadi titik awal yang perlu disesuaikan dengan kebijakan internal bersama unit keamanan informasi. Prosedur hukum yang rinci, seperti analisis dampak pelindungan data, disusun pada tahap implementasi.

## 2.4 Kelembagaan dan SOTK

Perpres Nomor 157 Tahun 2024 dan Peraturan Menteri Imigrasi dan Pemasyarakatan (Permen Imipas) Nomor 1 Tahun 2024 menetapkan organisasi dan tata kerja kementerian. Fungsi teknologi informasi keimigrasian yang dijalankan oleh DITTIK, bersama fungsi pengelolaan dan pemanfaatan data, menjadi tumpuan bagi SIMKIM dan pelaporan. Cakupan pembahasan terbatas pada Direktorat Jenderal Imigrasi, dengan sinergi lintas direktorat jenderal yang mengikuti prinsip Satu Data tingkat kementerian.

**Implikasi bagi tata kelola data**. Peran pemilik data, *steward* data, dan pengelola platform perlu dipetakan ke fungsi dalam susunan organisasi dan tata kerja (SOTK) yang sudah ada. Pemetaan tersebut bertujuan untuk menghindari terbentuknya struktur paralel di luar SOTK. Koordinasi antara DITTIK sebagai pengelola infrastruktur TIK dan fungsi pengelolaan serta pemanfaatan data merupakan syarat bagi kesiapan AI, bukan sekadar pilihan.

## 2.5 Keselarasan dengan Rencana Strategis Kementerian 2025–2029

Tata kelola data yang siap AI pada prinsipnya mendukung sasaran strategis peningkatan pelayanan publik digital, pengawasan dan penegakan hukum berbasis data, serta akuntabilitas penyelenggaraan. Dukungan tersebut diwujudkan melalui sasaran antara pada tingkat tata kelola, yaitu peran, klasifikasi, register, *lineage*, dan *checkpoint*, sehingga kontribusinya terhadap Rencana Strategis (Renstra) Kementerian 2025–2029 mudah ditelusuri pada tahap berikutnya. Pemetaan eksplisit ke sasaran program, indikator kinerja utama (IKU), dan indikator Renstra dilakukan setelah dokumen Renstra beserta turunan indikator resminya tersedia.

## 2.6 Kedudukan Dokumen dan Prinsip Penyelenggaraan

Subbab ini menjelaskan kedudukan kajian terhadap dokumen perencanaan lain serta prinsip yang dipegang dalam penyelenggaraan tata kelola data.

**Kedudukan dokumen**. Kajian ini berkedudukan sebagai kajian dan rekomendasi kebijakan dengan bentuk yang mendekati kerangka acuan kerja (KAK) untuk menaikkan kematangan tata kelola data sebagai syarat bagi analitik dan AI. Kedudukan tersebut tidak menggantikan Renstra, arsitektur SPBE instansi, maupun keputusan anggaran, melainkan menjadi rujukan arah bagi kebijakan internal manajemen data, klasifikasi, dan *checkpoint* pelaksanaan. Kajian ini melengkapi inisiatif *data warehouse* dan usulan penguatan platform AI, dengan penekanan pada celah tata kelola datanya. Spesifikasi platform di dalamnya bersifat konseptual dan netral terhadap penyedia. Rincian layanan awan tertentu menjadi materi lanjutan.

**Prinsip penyelenggaraan**. Tata kelola data diselenggarakan dengan berpegang pada pandangan bahwa data merupakan aset yang kepemilikan, kualitas, dan siklus hidupnya harus jelas. Standar dan metadata didahulukan atau setidaknya tumbuh seiring dengan pertumbuhan skala data. Keamanan dan PDP diterapkan sejak desain sehingga klasifikasi dan minimisasi melekat pada rantai data dan AI. Tata kelola juga harus mendahului perluasan AI, dalam arti investasi platform tidak mendahului fondasi data. Selain itu, *lineage* dan catatan audit tersedia untuk setiap angka dan keputusan sebagai wujud transparansi dan akuntabilitas, sementara kebijakan tidak dikunci terlalu dini pada merek layanan tertentu.

## 2.7 Literatur Ringkas dan Sintesis

Subbab ini merangkum literatur utama yang menjadi rujukan kajian beserta implikasinya bagi desain tata kelola data. Literatur tersebut dikelompokkan ke dalam empat tema berikut.

**Tata kelola data (DAMA-DMBOK).** DAMA International (2017), melalui kerangka DAMA-DMBOK edisi kedua (*Data Management Body of Knowledge*), menempatkan tata kelola data sebagai penentu arah bagi seluruh bidang manajemen data. Dalam kerangka tersebut, tata kelola mengarahkan bidang lain seperti kualitas data, metadata, keamanan data, serta data induk dan data referensi. Kajian ini menitikberatkan kepemilikan data dan *stewardship*, kualitas, metadata dan *lineage*, keamanan dan klasifikasi, serta pemakaian BI sebagai fondasi sebelum AI diperluas.

**Kematangan data pemerintahan.** Literatur penilaian kematangan data sektor pemerintah, antara lain Central Digital and Data Office (2023), menekankan bahwa kemajuan platform teknis tidak dengan sendirinya menaikkan kematangan apabila kebijakan, proses, dan kemampuan sumber daya manusia (SDM) tidak turut ditingkatkan. Kematangan data justru ditentukan oleh keseimbangan antara kesiapan teknologi dan kesiapan organisasi yang menggunakannya. Pandangan ini mendasari kerangka penilaian lima dimensi yang diusulkan dalam kajian ini.

**Sistem manajemen AI (ISO/IEC 42001).** Standar ISO/IEC 42001 yang diterbitkan ISO/IEC (2023) dirujuk sebagai arah kepatuhan bagi organisasi yang mengoperasikan AI, yang mencakup kebijakan, pengelolaan risiko, dan siklus hidup sistem AI, dan bukan sebagai *checklist* implementasi penuh. Implikasi praktisnya, pemanfaatan AI membutuhkan tata kelola dan kendali atas data masukannya.

**MLOps/AIOps.** Desain tim dan perangkat MLOps berada di luar cakupan kajian ini. Namun, efektivitas MLOps dalam menjalankan model AI secara berulang dan terkendali pada akhirnya bergantung pada data masukan yang terkurasi, terklasifikasi, berizin, dan dapat diaudit. Implikasi desain dari setiap landasan dirangkum pada tabel berikut.

| Landasan | Implikasi desain utama |
|----|----|
| Transformasi digital + SPBE | Integrasi, anti-silo, keamanan sejak awal; AI di atas fondasi |
| Satu Data | Standar, metadata, data induk/referensi, kejelasan peran data |
| PDP + BSSN/klasifikasi | Dasar pemrosesan, minimisasi, klasifikasi yang dijalankan, *guardrail* AI |
| SOTK/kelembagaan | Peran melekat pada fungsi resmi; koordinasi lintas fungsi |
| Renstra | Sasaran antara tata kelola; pemetaan IKU menyusul |
| DAMA + kematangan | Naikkan proses, data, dan tata kelola sebelum AI diperluas |
| ISO 42001 / MLOps | AI bertanggung jawab di atas data; bukan jalan pintas |

Sintesis tersebut menjadi dasar dalam membaca kondisi saat ini serta dalam merumuskan prinsip, arsitektur konseptual, rekomendasi tata kelola, dan kerangka penilaian kematangan.
