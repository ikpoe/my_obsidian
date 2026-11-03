# 1.3 Dasar Hukum dan Acuan

Blueprint ini mengikuti kerangka hukum yang sama dengan Peraturan Direksi Nomor 4/PERDIR.06/042025, yang telah mencantumkan dasar hukum tata kelola data dan TI BPJS Ketenagakerjaan pada bagian "Mengingat" (D1 hlm. 1–3). Acuan dikelompokkan menjadi lima bagian. Setiap kelompok menentukan aspek yang berbeda dari rancangan: kewenangan, kewajiban pelindungan, interoperabilitas, aturan internal, dan cara kerja.

## 1.3.1 Undang-Undang

Undang-undang menetapkan **kewenangan BPJS Ketenagakerjaan atas data** dan **kewajiban pelindungannya**.

- **UU Nomor 40 Tahun 2004** tentang Sistem Jaminan Sosial Nasional menjadi landasan program jaminan sosial ketenagakerjaan yang datanya dikelola ESDP. UU ini tidak tercantum dalam D1, tetapi dicantumkan sebagai landasan program.
- **UU Nomor 24 Tahun 2011** tentang Badan Penyelenggara Jaminan Sosial, sebagaimana telah diubah terakhir dengan UU Nomor 6 Tahun 2023, menetapkan BPJS Ketenagakerjaan sebagai badan hukum publik pengelola data peserta (D1 hlm. 2).
- **UU Nomor 27 Tahun 2022** tentang Pelindungan Data Pribadi mewajibkan pengendali data menetapkan dasar pemrosesan yang sah, membatasi tujuan, menjaga keamanan, mengatur retensi dan pemusnahan, serta menunjuk pejabat pelindungan data pribadi (D1 hlm. 2).
- **UU Nomor 11 Tahun 2008** tentang Informasi dan Transaksi Elektronik, sebagaimana telah diubah terakhir dengan UU Nomor 1 Tahun 2024, mengatur keandalan dan keamanan sistem elektronik (D1 hlm. 1–2).

## 1.3.2 Peraturan Pemerintah

Peraturan pemerintah merinci **cara memenuhi** kewajiban dalam undang-undang. Bagi Blueprint, kelompok ini menjadi sumber kunci waktu eksternal.

- **PP Nomor 33 Tahun 2026** tentang Peraturan Pelaksanaan UU Nomor 27 Tahun 2022 tentang Pelindungan Data Pribadi ditetapkan dan diundangkan pada 16 Juli 2026. PP ini mulai berlaku 6 bulan sejak diundangkan (Pasal 225), sehingga berlaku pada 16 Januari 2027 [perlu konfirmasi]. PP ini mengatur antara lain retensi dan minimisasi data. Tanggal berlakunya jatuh pada awal Tahun 1 peta jalan.
- **PP Nomor 71 Tahun 2019** tentang Penyelenggaraan Sistem dan Transaksi Elektronik mengatur kewajiban penyelenggara sistem elektronik, termasuk keandalan dan pemulihan sistem (D1 hlm. 2).
- **PP Nomor 85 Tahun 2013** tentang Tata Cara Hubungan Antar Lembaga BPJS menjadi dasar kerja sama dan pertukaran data dengan kementerian/lembaga (D1 hlm. 2).

## 1.3.3 Kebijakan Data Nasional

Kebijakan nasional menetapkan **standar interoperabilitas** yang harus dipenuhi oleh data yang dipertukarkan BPJS Ketenagakerjaan dengan pemerintah.

- **Perpres Nomor 39 Tahun 2019** tentang Satu Data Indonesia mengatur prinsip standar data, metadata, interoperabilitas, dan kode referensi (D1 hlm. 2).
- **Inpres Nomor 4 Tahun 2025** tentang Data Tunggal Sosial dan Ekonomi Nasional mendorong penyelarasan identitas dan data sosial-ekonomi lintas lembaga (D1 hlm. 3).
- **Perpres Nomor 95 Tahun 2018** tentang Sistem Pemerintahan Berbasis Elektronik dan **Perpres Nomor 82 Tahun 2023** tentang Percepatan Transformasi Digital dan Keterpaduan Layanan Digital Nasional dipakai sebagai acuan pelengkap. Keduanya tidak tercantum dalam D1, dan keberlakuannya bagi BPJS Ketenagakerjaan sebagai badan hukum publik perlu dipastikan [perlu konfirmasi].

## 1.3.4 Ketentuan Sektor dan Internal

Ketentuan internal menjadi **acuan langsung** Blueprint. Blueprint tidak membuat kebijakan baru, tetapi menerjemahkan kebijakan yang sudah ada ke dalam arsitektur.

- **Peraturan Direksi Nomor 4/PERDIR.06/042025** tentang Pedoman Tata Kelola Data dan Teknologi Informasi, ditetapkan 23 April 2025, menyatukan empat peraturan sebelumnya ke dalam satu pedoman (D1 hlm. 1, 62). Bagian yang paling banyak dirujuk:
  - struktur tata kelola dari KPTI hingga kantor cabang sebagai produsen data (D1 hlm. 7–10);
  - arsitektur dan pengelolaan data (D1 hlm. 14–17);
  - keamanan informasi, termasuk CISO dan klasifikasi data (D1 hlm. 39–52);
  - kolaborasi data dengan pihak eksternal (Bab VI, D1 hlm. 53–58). Setiap PKS wajib memuat tujuan, metode integrasi, elemen data, dan aspek keamanan;
  - adendum PKS yang sudah berjalan paling lambat 6 bulan sejak penetapan, yaitu sekitar Oktober 2025 (D1 hlm. 61–62).
- **Prosedur Pengelolaan Kualitas Data** Nomor 4.009/SOP/MDT/082024 versi 0.1, disusun Bidang Data Governance dengan pemilik Deputi Bidang Manajemen Data dan Analitik (D6 hlm. 1). Prosedur ini menjabarkan siklus kualitas data ke tingkat operasional:
  - penetapan metrik dan *critical data element* (CDE), profiling, cleansing, penilaian, dan tindak lanjut (D6 hlm. 4, 10–12);
  - indeks kualitas data peserta beserta tindak lanjut per tingkat (D6 hlm. 30–31);
  - peran Data Quality, Data Liaison, dan Business Owner, termasuk persetujuan laporan kualitas data bulanan (D6 hlm. 5, 29).

- **Peraturan OJK** yang dirujuk D1, terutama POJK Nomor 4/POJK.05/2021 tentang Penerapan Manajemen Risiko dalam Penggunaan Teknologi Informasi oleh Lembaga Jasa Keuangan Nonbank (D1 hlm. 3). Penerapannya pada BPJS Ketenagakerjaan perlu dipastikan [perlu konfirmasi].

## 1.3.5 Kerangka Kerja dan Dokumen Studi

Penilaian kondisi saat ini dan penyusunan rancangan dalam Blueprint ini mengacu pada kerangka kerja manajemen data yang lazim digunakan.

- **DAMA-DMBOK** menjadi rujukan dalam menilai kematangan pengelolaan data, meninjau kesenjangan, serta menentukan cakupan topik pada setiap area arsitektur. Sumber (buku): DAMA International, *DAMA-DMBOK: Data Management Body of Knowledge*, edisi ke-2, Technics Publications, 2017, ISBN 978-1-63462-234-9 [perlu konfirmasi edisi].

Dokumen studi yang menjadi sumber fakta kondisi saat ini adalah sebagai berikut.

- **D1** Perdir 4/PERDIR.06/042025;
- **D2** paparan Arsitektur Enterprise kondisi saat ini;
- **D3** tangkapan layar paparan BPJS Ketenagakerjaan (status kapabilitas platform, POC MDM, model konseptual);
- **D4** dokumen kebijakan/teknis API Data Hub;
- **D6** Prosedur Pengelolaan Kualitas Data (4.009/SOP/MDT/082024);
- **R1** rekaman diskusi dengan tim BPJS Ketenagakerjaan;
- **Laporan Terintegrasi BPJS Ketenagakerjaan 2024 dan 2025** untuk data kepesertaan dan konteks organisasi.

## 1.3.6 Implikasi terhadap Blueprint

Setiap acuan di atas berpengaruh langsung terhadap rancangan arsitektur data. Tabel berikut merangkum hal pokok yang perlu diakomodasi dari tiap acuan beserta area Blueprint yang menjabarkannya.

| Acuan                                   | Implikasi utama                                                                                                                                  | Area / bagian   |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| UU 24/2011                              | BPJS Ketenagakerjaan memegang kewenangan dan akuntabilitas atas data peserta                                                                     | 4.1, 4.6        |
| UU 27/2022 + PP 33/2026                 | Klasifikasi, dasar pemrosesan, masking, retensi, dan pemusnahan data pribadi; kunci waktu 16 Jan 2027 [perlu konfirmasi]                         | 4.4, 4.8, 3.4.4 |
| UU 11/2008 + PP 71/2019                 | Keandalan sistem, pemulihan, dan jejak audit                                                                                                     | 4.2, 4.4, 4.8   |
| PP 85/2013                              | Pertukaran data antarlembaga melalui perjanjian kerja sama                                                                                       | 4.5             |
| Perpres 39/2019 + Inpres 4/2025         | Standar data, metadata, kode referensi, dan identitas tunggal                                                                                    | 4.3, 4.6, 4.7   |
| Perdir 4/2025                           | Peran, forum, klasifikasi, standar kolaborasi, dan penyesuaian PKS                                                                               | 4.1–4.10, BAB V |
| POJK 4/POJK.05/2021 [perlu konfirmasi]  | Manajemen risiko TI                                                                                                                              | 4.2, 5.2        |
| Prosedur Pengelolaan Kualitas Data (D6) | Metrik, CDE, indeks kualitas data peserta, dan peran Data Quality, Data Liaison, serta Business Owner menjadi titik awal rancangan kualitas data | 4.1, 4.7, 4.10  |
| DAMA-DMBOK                              | Kerangka fungsi manajemen data sebagai acuan pembagian area dan praktik baik                                                                     | 4.1–4.10        |

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Integrasi D6 (2026-11-02):** Prosedur Pengelolaan Kualitas Data ditambahkan pada 1.3.4 (ketentuan internal), daftar dokumen studi 1.3.5 (kode D6), dan tabel 1.3.6. **Tindak lanjut:** skala indeks kualitas data di D6 belum konsisten, yaitu tiga tingkat di batang tubuh (hlm. 30) dan lima grade di lampiran (hlm. 60). Perlu dikonfirmasi ke BPJS Ketenagakerjaan. **Tindak lanjut (prioritas rendah):** dasar hukum D6 masih merujuk Pedoman Tata Kelola Data 2022 yang telah dicabut Perdir 4/2025 (D6 hlm. 4; D1 hlm. 62). Dapat ditanyakan saat ada kesempatan.

**Keputusan yang perlu diambil ikpu**
1. **Selesai (2026-11-02):** rincian regulasi dari 1.1 telah dipindahkan ke subbab ini. Subbab 1.1 kini hanya merujuk ke 1.3.
2. UU 40/2004 tetap dicantumkan (diputuskan ikpu 2026-11-02; judul sudah diverifikasi ke JDIH BPK). Masih terbuka: apakah Perpres 95/2018 dan 82/2023 (SPBE) tetap dicantumkan sebagai acuan pelengkap atau dihapus.
3. Pencantuman POJK. D1 mencantumkan tiga POJK (13/2018, 4/2021, 6/2022). Draf ini hanya menyebut POJK 4/2021 karena paling relevan dengan arsitektur.
4. [Selesai 2026-11-02: ISO dan COBIT dikeluarkan dari 1.3.] D1 juga merujuk satu kerangka manajemen data lain di samping COBIT (hlm. 6). Kerangka itu sengaja tidak disebut.
5. Tidak ada gambar baru. Tabel 1.3.6 dinilai sudah cukup. Linimasa regulasi tersedia di Gambar 1.3.

**Konfirmasi ke klien atau verifikasi**
1. Tanggal berlaku PP 33/2026. Teks Pasal 225 dan tanggal pengundangan 16 Juli 2026 sudah diverifikasi dari salinan PP. Penafsiran tanggal berlaku 16 Januari 2027 perlu dikonfirmasi ke bagian hukum.
2. Keberlakuan SPBE (Perpres 95/2018 dan 82/2023) dan POJK bagi BPJS Ketenagakerjaan.
3. [Tidak relevan lagi untuk 1.3, ISO dikeluarkan 2026-11-02] Status sertifikasi ISO/IEC 27001 dan ruang lingkupnya, apakah mencakup ESDP.
4. Nomor dan halaman D4 (dokumen tidak bernomor halaman).
5. Status adendum PKS sesuai ketentuan peralihan Perdir 4/2025 (dipindahkan dari 1.1).
