# 1.4 Metodologi dan Sumber Data

Blueprint ini disusun berdasarkan bukti yang dikumpulkan melalui kajian dokumen kebijakan dan arsitektur, diskusi dengan tim BPJS Ketenagakerjaan, serta publikasi resmi. Setiap temuan tentang kondisi saat ini ditelusuri ke dokumen atau diskusi, lalu dibandingkan dengan kebijakan internal dan regulasi yang berlaku. Rancangan target dan peta jalan dirumuskan setelah kesenjangan tersebut teridentifikasi.

## 1.4.1 Tahapan Penyusunan

Penyusunan Blueprint berjalan dalam enam tahap berurutan. Validasi bersama BPJS Ketenagakerjaan dilakukan pada setiap tahap (Gambar 1.6).

![[G1-6-Tahapan-Metodologi.png]]
*Gambar 1.6 Tahapan Penyusunan Blueprint*

**Kajian dokumen.** Tahap pertama membedakan dua sudut pandang. Peraturan Direksi Nomor 4/PERDIR.06/042025 (D1) dibaca sebagai **kondisi seharusnya**, yaitu cara data dan TI semestinya dikelola menurut kebijakan BPJS Ketenagakerjaan sendiri. Paparan Arsitektur Enterprise kondisi saat ini (D2) dibaca sebagai **kondisi saat ini**. Untuk area kualitas data, Prosedur Pengelolaan Kualitas Data (D6) juga dibaca sebagai kondisi seharusnya karena merinci D1 hingga tingkat langkah dan peran. Ketiganya dilengkapi dengan dua dokumen lain:

- tangkapan layar paparan BPJS Ketenagakerjaan pada 31 Oktober 2026 (D3), berisi status kapabilitas platform, POC MDM, dan model konseptual;
- dokumen kebijakan/teknis API Data Hub (D4).

**Diskusi dengan tim BPJS Ketenagakerjaan.** Dokumen tidak selalu menggambarkan praktik sehari-hari. Karena itu, temuan dari kajian dokumen dikonfirmasi melalui diskusi dengan tim BPJS Ketenagakerjaan pada 27 Oktober 2026 (R1). Diskusi ini memperjelas praktik yang tidak tertulis, misalnya cara replikasi data berjalan, kunci identitas yang dipakai, dan pengendalian akses yang sudah diterapkan.

**Reviu per area pengelolaan data.** Setiap area pengelolaan data direviu untuk membangun pemahaman yang utuh tentang kondisinya. DAMA-DMBOK dipakai untuk menentukan hal yang perlu diperiksa pada setiap area, sehingga tidak ada aspek penting yang terlewat. Aturan reviunya:

- setiap temuan harus didukung bukti dari dokumen atau diskusi;
- kebijakan dan praktik ditinjau terpisah;
- kebijakan yang sudah ada tetapi belum dijalankan tidak dianggap sudah berjalan.

**Analisis kesenjangan.** Hasil reviu dibandingkan melalui tiga sudut pandang:

- **kondisi saat ini**, yaitu praktik yang berjalan dan ketentuan Perdir 4/2025;
- **regulasi**, terutama pelindungan data pribadi dan Satu Data Indonesia;
- **kondisi ideal**, berdasarkan DAMA-DMBOK dan praktik yang lazim digunakan.

Kesenjangan yang muncul di beberapa area sekaligus dirumuskan sebagai tantangan utama.

**Perumusan prinsip dan arsitektur target.** Prinsip arsitektur data dirumuskan dari tantangan tersebut. Prinsip ini menjadi dasar rancangan target pada setiap area. Untuk keputusan yang memiliki lebih dari satu pilihan, misalnya pola pemisahan lingkungan platform, beberapa opsi dibandingkan dengan kriteria yang sama sebelum satu opsi direkomendasikan.

**Penyusunan peta jalan.** Inisiatif disusun per kuartal selama tiga tahun. Urutannya ditentukan oleh dua hal:

- **ketergantungan antarinisiatif**, karena langkah yang menjadi prasyarat harus selesai lebih dulu;
- **kunci waktu eksternal**, seperti tanggal berlaku peraturan pelaksana UU Pelindungan Data Pribadi.

**Validasi dan reviu.** Setiap hasil tahap dipaparkan dan direviu bersama tim BPJS Ketenagakerjaan. Masukan dari reviu dipakai untuk memperbaiki temuan dan rancangan sebelum tahap berikutnya dimulai.

## 1.4.2 Sumber Data

Sumber yang digunakan dikelompokkan sebagai berikut:

- **Regulasi:** undang-undang, peraturan pemerintah, dan kebijakan data nasional yang berkaitan dengan jaminan sosial, sistem elektronik, dan pelindungan data pribadi.
- **Kebijakan internal:** Perdir 4/PERDIR.06/042025 (D1) dan Prosedur Pengelolaan Kualitas Data 4.009/SOP/MDT/082024 (D6).
- **Dokumen klien:** paparan Arsitektur Enterprise kondisi saat ini (D2), tangkapan layar paparan BPJS Ketenagakerjaan (D3), dan dokumen kebijakan/teknis API Data Hub (D4).
- **Rekaman diskusi:** diskusi dengan tim BPJS Ketenagakerjaan (R1).
- **Publikasi resmi:** Laporan Terintegrasi BPJS Ketenagakerjaan 2024 dan 2025, untuk angka kepesertaan dan konteks organisasi.
- **Kerangka kerja:** buku DAMA-DMBOK, sebagai rujukan reviu dan cakupan topik per area.

Setiap rujukan ditulis dengan kode sumber dan nomor halaman tercetak, misalnya "(D1 hlm. 53–58)". Dokumen tanpa nomor halaman cukup dirujuk dengan kodenya. Sebagian dokumen teknis mitra bersifat terbatas, sehingga yang dikutip hanya pola temuan umum, tanpa rincian teknis, alamat sistem, maupun kredensial.

Hasil diskusi diperlakukan sebagai bukti pendukung dokumen. Bila keduanya bertentangan, kedua versi dicantumkan dan ditandai [perlu konfirmasi]. Penanda yang sama dipakai untuk fakta yang hanya bersumber dari satu pihak atau yang bergantung pada asumsi yang belum disepakati. Fakta bertanda ini tidak dijadikan dasar tunggal suatu rekomendasi.

## 1.4.3 Keterbatasan

Blueprint ini memiliki beberapa keterbatasan:

- Reviu didasarkan pada dokumen dan diskusi, bukan pada pengujian langsung terhadap sistem, data, atau konfigurasi. Kondisi sebenarnya dapat berbeda dari yang tercatat dalam dokumen.
- Sebagian fakta masih menunggu konfirmasi, antara lain status pusat data cadangan, hasil POC MDM, dan tanggal mulai Tahun 1 peta jalan.
- Jumlah narasumber dalam diskusi masih terbatas. Praktik di unit kerja dan kantor cabang yang tidak terwakili belum tentu tergambar.
- Hasil reviu merupakan pandangan tim ahli berdasarkan data yang diperoleh selama proses reviu.

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Integrasi D6 (2026-11-02):** D6 ditambahkan pada tahap kajian dokumen (sebagai kondisi seharusnya untuk area kualitas data) dan pada kelompok kebijakan internal di 1.4.2.

> **Catatan aset:** `assets/G1-6-Tahapan-Metodologi.svg` (sumber yang dapat disunting; teks asli, grup `<g>` ber-id) dan `.png` (ekspor 220 dpi).

**Keputusan yang perlu diambil ikpu**
1. **Penomoran gambar tidak berurutan.** Subbab 1.1 kini memakai Gambar 1.1–1.2 (Gambar 1.3 linimasa tidak lagi disematkan), sedangkan 1.2 memakai Gambar 1.4–1.5, dan subbab ini Gambar 1.6. Perlu diputuskan apakah semua gambar dinomori ulang setelah BAB I stabil, dan apakah file G1-3 dipakai di 1.3 atau dihapus.
2. Validasi digambarkan berulang pada setiap tahap. Pilihannya mempertahankan ini atau menjadikannya tahap terakhir saja jika reviu klien hanya dilakukan sekali.
3. Penyebutan contoh opsi "pola pemisahan lingkungan platform" pada tahap perumusan arsitektur target. Perlu dipastikan contoh ini sesuai dengan perbandingan opsi yang benar-benar disajikan.
4. Skala kematangan tidak lagi disebut karena skor tidak dipublikasikan. Judul Lampiran A (Penilaian Kematangan per Area) perlu disesuaikan.
5. Dokumen teknis mitra yang bersifat terbatas hanya disebut secara umum dan tidak diberi kode, karena tidak tercantum dalam daftar dokumen studi di 1.3.

**Konfirmasi ke klien atau verifikasi**
1. Tanggal diskusi R1 (27 Oktober 2026) dan tanggal paparan D3 (31 Oktober 2026).
2. Daftar narasumber atau unit yang terlibat dalam diskusi, bila perlu dicantumkan.
3. Apakah klien menghendaki reviu formal (misalnya sesi validasi temuan) sebelum Blueprint final.
