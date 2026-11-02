# 1.3 Dasar Hukum dan Acuan

Blueprint ini mengikuti kerangka hukum yang sama dengan Peraturan Direksi Nomor 4/PERDIR.06/042025, yang telah mencantumkan dasar hukum tata kelola data dan TI BPJS Ketenagakerjaan pada bagian "Mengingat" (D1 hlm. 1–3). Acuan dikelompokkan menjadi lima bagian. Setiap kelompok menentukan aspek yang berbeda dari rancangan: kewenangan, kewajiban pelindungan, interoperabilitas, aturan internal, dan cara kerja.

## 1.3.1 Undang-Undang

Undang-undang menetapkan **kewenangan BPJS Ketenagakerjaan atas data** dan **kewajiban pelindungannya**.

- **UU Nomor 40 Tahun 2004** tentang Sistem Jaminan Sosial Nasional menjadi landasan program jaminan sosial ketenagakerjaan yang datanya dikelola ESDP. UU ini tidak tercantum dalam D1 [perlu konfirmasi apakah dicantumkan].
- **UU Nomor 24 Tahun 2011** tentang Badan Penyelenggara Jaminan Sosial, sebagaimana telah diubah terakhir dengan UU Nomor 6 Tahun 2023, menetapkan BPJS Ketenagakerjaan sebagai badan hukum publik pengelola data peserta (D1 hlm. 2).
- **UU Nomor 27 Tahun 2022** tentang Pelindungan Data Pribadi mewajibkan dasar pemrosesan yang sah, pembatasan tujuan, keamanan, retensi, dan pemusnahan data pribadi (D1 hlm. 2).
- **UU Nomor 11 Tahun 2008** tentang Informasi dan Transaksi Elektronik, sebagaimana telah diubah terakhir dengan UU Nomor 1 Tahun 2024, mengatur keandalan dan keamanan sistem elektronik (D1 hlm. 1–2).

## 1.3.2 Peraturan Pemerintah

Peraturan pemerintah merinci **cara memenuhi** kewajiban dalam undang-undang. Bagi Blueprint, kelompok ini menjadi sumber kunci waktu eksternal.

- **PP Nomor 33 Tahun 2026** tentang Peraturan Pelaksanaan UU Nomor 27 Tahun 2022 tentang Pelindungan Data Pribadi ditetapkan dan diundangkan pada 16 Juli 2026. PP ini mulai berlaku 6 bulan sejak diundangkan (Pasal 225), sehingga berlaku pada 16 Januari 2027 [perlu konfirmasi]. Tanggal ini jatuh pada awal Tahun 1 peta jalan.
- **PP Nomor 71 Tahun 2019** tentang Penyelenggaraan Sistem dan Transaksi Elektronik mengatur kewajiban penyelenggara sistem elektronik, termasuk keandalan dan pemulihan sistem (D1 hlm. 2).
- **PP Nomor 85 Tahun 2013** tentang Tata Cara Hubungan Antar Lembaga BPJS menjadi dasar kerja sama dan pertukaran data dengan kementerian/lembaga (D1 hlm. 2).

## 1.3.3 Kebijakan Data Nasional

Kebijakan nasional menetapkan **standar interoperabilitas** yang harus dipenuhi oleh data yang dipertukarkan BPJS Ketenagakerjaan dengan pemerintah.

- **Perpres Nomor 39 Tahun 2019** tentang Satu Data Indonesia mengatur prinsip standar data, metadata, interoperabilitas, dan kode referensi (D1 hlm. 2).
- **Inpres Nomor 4 Tahun 2025** tentang Data Tunggal Sosial dan Ekonomi Nasional mendorong penyelarasan identitas dan data sosial-ekonomi lintas lembaga (D1 hlm. 3).
- **Perpres Nomor 95 Tahun 2018** tentang Sistem Pemerintahan Berbasis Elektronik dan **Perpres Nomor 82 Tahun 2023** tentang Percepatan Transformasi Digital dan Keterpaduan Layanan Digital Nasional dipakai sebagai acuan pelengkap. Keduanya tidak tercantum dalam D1, dan keberlakuannya bagi BPJS Ketenagakerjaan sebagai badan hukum publik perlu dipastikan [perlu konfirmasi].

## 1.3.4 Ketentuan Sektor dan Internal

Ketentuan internal menjadi **acuan langsung** Blueprint. Blueprint tidak membuat kebijakan baru, tetapi menerjemahkan kebijakan yang sudah ada ke dalam arsitektur.

- **Peraturan Direksi Nomor 4/PERDIR.06/042025** tentang Pedoman Tata Kelola Data dan Teknologi Informasi, ditetapkan 23 April 2025 (D1). Bagian yang paling banyak dirujuk:
  - tata kelola dan peran, termasuk KPTI (D1 hlm. 7–10);
  - arsitektur dan pengelolaan data (D1 hlm. 14–17);
  - keamanan informasi, termasuk CISO dan klasifikasi data (D1 hlm. 39–52);
  - kolaborasi data dengan pihak eksternal (Bab VI, D1 hlm. 53–58);
  - penyesuaian PKS paling lambat 6 bulan (D1 hlm. 61–62).
- **Peraturan OJK** yang dirujuk D1, terutama POJK Nomor 4/POJK.05/2021 tentang Penerapan Manajemen Risiko dalam Penggunaan Teknologi Informasi oleh Lembaga Jasa Keuangan Nonbank (D1 hlm. 3). Penerapannya pada BPJS Ketenagakerjaan perlu dipastikan [perlu konfirmasi].

## 1.3.5 Standar, Kerangka Kerja, dan Dokumen Studi

Standar dan kerangka kerja dipakai sebagai **acuan praktik baik**, bukan sebagai kewajiban hukum.

- **ISO/IEC 27001** untuk sistem manajemen keamanan informasi. D1 merujuk "persyaratan ISO yang ditetapkan" dalam pemantauan layanan TI (D1 hlm. 28) [perlu konfirmasi status sertifikasi].
- **COBIT** sebagai kerangka tata kelola TI yang dirujuk D1 (D1 hlm. 6).

Dokumen studi yang menjadi sumber fakta kondisi saat ini adalah sebagai berikut. Metode penggunaannya diuraikan pada subbab 1.4.

- **D1** Perdir 4/PERDIR.06/042025;
- **D2** paparan Arsitektur Enterprise kondisi saat ini;
- **D3** tangkapan layar paparan BPJS Ketenagakerjaan (status kapabilitas platform, POC MDM, model konseptual);
- **D4** dokumen kebijakan/teknis API Data Hub;
- **R1** rekaman diskusi dengan tim BPJS Ketenagakerjaan;
- **Laporan Terintegrasi BPJS Ketenagakerjaan 2024 dan 2025** untuk data kepesertaan dan konteks organisasi.

## 1.3.6 Implikasi terhadap Blueprint

| Acuan | Implikasi utama | Area / bagian |
|---|---|---|
| UU 24/2011 | BPJS Ketenagakerjaan memegang kewenangan dan akuntabilitas atas data peserta | 4.1, 4.6 |
| UU 27/2022 + PP 33/2026 | Klasifikasi, dasar pemrosesan, masking, retensi, dan pemusnahan data pribadi; kunci waktu 16 Jan 2027 [perlu konfirmasi] | 4.4, 4.8, 3.4.4 |
| UU 11/2008 + PP 71/2019 | Keandalan sistem, pemulihan, dan jejak audit | 4.2, 4.4, 4.8 |
| PP 85/2013 | Pertukaran data antarlembaga melalui perjanjian kerja sama | 4.5 |
| Perpres 39/2019 + Inpres 4/2025 | Standar data, metadata, kode referensi, dan identitas tunggal | 4.3, 4.6, 4.7 |
| Perdir 4/2025 | Peran, forum, klasifikasi, standar kolaborasi, dan penyesuaian PKS | 4.1–4.10, BAB V |
| POJK 4/POJK.05/2021 [perlu konfirmasi] | Manajemen risiko TI | 4.2, 5.2 |
| ISO/IEC 27001, COBIT | Acuan kendali keamanan dan tata kelola TI | 4.1, 4.8 |

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

**Keputusan yang perlu diambil ikpu**
1. Rincian regulasi di 1.1 (paragraf UU PDP, PP 33/2026, Satu Data) dapat dipindahkan ke subbab ini, sehingga 1.1 cukup merujuk ke 1.3.
2. Pencantuman acuan di luar D1: UU 40/2004, Perpres 95/2018, dan Perpres 82/2023. Pilihannya tetap dicantumkan sebagai acuan pelengkap atau dihapus.
3. Pencantuman POJK. D1 mencantumkan tiga POJK (13/2018, 4/2021, 6/2022). Draf ini hanya menyebut POJK 4/2021 karena paling relevan dengan arsitektur.
4. D1 juga merujuk satu kerangka manajemen data lain di samping COBIT (hlm. 6). Kerangka itu sengaja tidak disebut.
5. Tidak ada gambar baru. Tabel 1.3.6 dinilai sudah cukup. Linimasa regulasi tersedia di Gambar 1.3.

**Konfirmasi ke klien atau verifikasi**
1. Tanggal berlaku PP 33/2026. Teks Pasal 225 dan tanggal pengundangan 16 Juli 2026 sudah diverifikasi dari salinan PP. Penafsiran tanggal berlaku 16 Januari 2027 perlu dikonfirmasi ke bagian hukum.
2. Keberlakuan SPBE (Perpres 95/2018 dan 82/2023) dan POJK bagi BPJS Ketenagakerjaan.
3. Status sertifikasi ISO/IEC 27001 dan ruang lingkupnya, apakah mencakup ESDP.
4. Nomor dan halaman D4 (dokumen tidak bernomor halaman).
