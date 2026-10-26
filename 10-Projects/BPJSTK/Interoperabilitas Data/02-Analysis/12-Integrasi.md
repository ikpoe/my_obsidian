---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Integrasi & Interoperabilitas Data
sesi: 2
blueprint: [3.4, 5.2, 5.3, 5.4, 7.2]
versi: 2 (struktur upstream–downstream, 26 Okt 2026)
status: draft-diskusi
acuan_internal: DAMA-DMBOK v2, bab Data Integration & Interoperability
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, 00-Analisis-Dokumen-BPJS.md, 01-Inventori-Tender-BPJSTK.csv, riset publik]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 77 = cetak '- 76 -')."
---

# Sesi 2 — Integrasi & Interoperabilitas Data

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI (kebijakan, kondisi yang seharusnya). **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (paparan kondisi saat ini/as-is). "hlm." merujuk pada nomor halaman PDF.

> **Kerangka sesi.** Platform data (ESDP) menjadi poros. Integrasi adalah salah satu komponen ESDP dan dinilai dari dua sisi: **upstream** (sumber data → platform) dan **downstream** (platform → pengguna data). Integrasi operasional antar-aplikasi (SMILE ke aplikasi lain, H2H di luar platform) berada di luar lingkup inti, tetapi wajib mengikuti standar dan prinsip yang sama.

## 1. Ringkasan
- **Upstream sudah punya fondasi tool, tetapi belum dikelola sebagai layanan.** Oracle ADG (standby sebagai Data Staging), Talend (ETL dari MariaDB, Oracle, HDFS), dan Qlik Replicate (CDC) ke Data Lake Cloudera sudah berjalan (D2 hlm. 20–21). Belum ada bukti ukuran kelengkapan ingest, SLA freshness data, data contract dengan pemilik sumber, atau rekonsiliasi. Baru sebagian Data Building Block yang tersimpan lengkap dan terstruktur (D2 hlm. 21).
- **Downstream adalah sisi terlemah.** Konsumsi internal masih bergantung pada permintaan ke MDT yang memakan waktu (D2 hlm. 22). Distribusi ke mitra memakai API dan SFTP bulk untuk PKS K/L (D2 hlm. 21), dan Data Collaboration memakai platform eksternal yang beragam (D2 hlm. 22). Belum ada standar API, katalog layanan data, atau SLA konsumsi. Distribusi skor/insight ke aplikasi internal tidak terlihat di dokumen.
- **Kebijakan kuat di sisi keamanan dan PKS, kosong di sisi arsitektur.** Ps. 66 mengatur otorisasi, PKS/NDA/IP whitelist, dan membatasi teknologi pada web service dan SFTP (D1 hlm. 50–51). Ps. 69–73 mengatur skema kolaborasi dan isi PKS (D1 hlm. 54–58). Standar API, data contract, SLA interface, dan pemantauan tidak diatur.
- **Integrasi operasional di luar platform tetap berisiko.** SMILE menjadi hub integrasi yang berpotensi SPOF, masih ada Direct-to-DB (D2 hlm. 39), dan integrasi eksternal belum seragam dan belum terpusat (D2 hlm. 40). Area ini tidak dinilai sebagai lingkup inti, tetapi standar yang dirumuskan harus dapat dipakai untuknya.
- **Skor usulan: upstream ±1,6; downstream ±1,6.** Praktik ada di beberapa jalur, tetapi tanpa standar, kontrak, atau pengukuran.
- **Rekomendasi:** Blueprint menetapkan **prinsip integrasi terstandar** untuk ESDP, dengan pola dan kontrol berbeda per sisi. Upstream berfokus pada CDC-first, data contract dengan pemilik sumber, dan beban minimum pada sumber. Downstream berfokus pada API gateway, standar API, kontrol akses berbasis klasifikasi, SLA, dan keterkaitan dengan PKS. Prinsip ini juga berlaku bagi integrasi operasional dan dapat menjadi dasar revisi Ps. 66.

## 2. Peta integrasi saat ini

### 2.1 Upstream (sumber → platform)

| Sumber                                                                     | Pola                          | Teknologi                | Bukti                        | Catatan                                                                                                                                                                                           |
| -------------------------------------------------------------------------- | ----------------------------- | ------------------------ | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SMILE dan aplikasi berbasis Oracle                                         | Replikasi database ke standby | Oracle Active Data Guard | D2 hlm. 21                   | Standby dipakai sebagai Data Staging (D2 hlm. 21). Kaitan dengan ODS di D1 hlm. 77 [perlu konfirmasi]                                                                                             |
| Database lain (MariaDB, Oracle, HDFS)                                      | Batch/ETL                     | Talend                   | D2 hlm. 21                   | Jadwal, jumlah job, dan cakupan tabel tidak disebut. Database as-is juga mencakup Yugabyte, SQL Server, PostgreSQL (D2 hlm. 37); cara ingest-nya [perlu konfirmasi]                               |
| Sumber operasional ke Data Lake                                            | CDC                           | Qlik Replicate           | D2 hlm. 21                   | Sumber dan tabel yang dicakup [perlu konfirmasi]                                                                                                                                                  |
| Sumber eksternal (Dukcapil, DJP, Kemnaker SIAPkerja, BPJS Kesehatan, dll.) | API / SFTP bulk               | API, SFTP                | D2 hlm. 21, 40; riset publik | D2 menyatakan sistem sudah mengakomodasi data eksternal, dengan risiko format berbeda (D2 hlm. 21). Apakah data eksternal masuk ke platform atau hanya ke aplikasi operasional [perlu konfirmasi] |
| Data tidak terstruktur                                                     | –                             | Cloudera (kapabilitas)   | D2 hlm. 20                   | Kapabilitas belum digunakan secara operasional (D2 hlm. 20)                                                                                                                                       |

### 2.2 Downstream (platform → pengguna data)

| Konsumen | Pola | Teknologi | Bukti | Catatan |
|---|---|---|---|---|
| Manajemen & unit bisnis (dashboard/BI) | Data mart → dashboard & report | Tableau, QlikSense; 17 dashboard | D2 hlm. 22 | Kebutuhan analisis lewat request ke MDT, butuh waktu lama (D2 hlm. 22). Self-service belum tampak, padahal menjadi prinsip D1 (D1 hlm. 76) |
| Analis internal | Analitik lanjutan | Python, KNIME | D2 hlm. 22 | – |
| Aplikasi internal (skor, insight) | Tidak terdokumentasi | – | – | Tidak ada bukti platform mengirim hasil analitik kembali ke aplikasi operasional [perlu konfirmasi] |
| Mitra K/L | API; SFTP bulk berbasis PKS | API, SFTP | D2 hlm. 21; D1 hlm. 51 | Apakah dilayani dari platform atau langsung dari aplikasi operasional [perlu konfirmasi] |
| Mitra kolaborasi / data-as-a-service | Platform eksternal beragam | Tidak dinamai | D2 hlm. 22; D1 hlm. 54 | Kepatuhan terhadap Ps. 66(4) [perlu konfirmasi] |

### 2.3 Integrasi operasional (di luar lingkup inti, acuan standar)

| Dimensi | Kondisi | Bukti |
|---|---|---|
| Internal | API, Middleware/WS, Direct-to-DB; SMILE sebagai hub, potensi SPOF | D2 hlm. 39 |
| Eksternal | Host-to-Host dan API; belum seragam; tiap aplikasi berintegrasi sendiri | D2 hlm. 40 |
| Mitra eksternal di diagram | Dukcapil, SISKOP2MI, Mitra Pajakku, SiapKerja, bank, payment gateway, kanal mitra | D2 hlm. 40 |
| API layer | WAF/API Security, API Management (D2 hlm. 37); API Gateway (D2 hlm. 50); produk tidak dinamai; riset publik hanya menemukan tender keamanan API 2024 | D2 hlm. 37, 50 |

## 3. Tabel penilaian

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal. Setiap skor harus punya bukti.

### 3.1 Upstream

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| US-1. Cakupan & kelengkapan ingest | Semua sumber prioritas terdaftar dan masuk platform; kelengkapan diukur | Arsitektur DWBI wajib menjelaskan dari mana, ke mana, kapan, dan bagaimana data diintegrasikan (D1 hlm. 76) | Baru sebagian Data Building Block tersimpan lengkap dan terstruktur (D2 hlm. 21). Sebagian aplikasi belum terhubung ke SMILE (D2 hlm. 45) | Tidak ada daftar sumber ter-ingest maupun ukuran kelengkapan | 2 |
| US-2. Pola & tool ingest | Pola standar per jenis sumber (CDC, batch, file), tool terkelola | Komponen data integration (ETL/virtualization) dan staging (D1 hlm. 77) | ADG → staging, Talend, Qlik Replicate (D2 hlm. 21). Tender Qlik/Talend terkonfirmasi (inventori tender) | Praktik konsisten di jalur Oracle, tetapi aturan pemilihan pola tidak terdokumentasi; cakupan database lain [perlu konfirmasi] | 2 (calon 3) |
| US-3. freshness data & SLA ingest | Target latensi per domain, dipantau | Tidak diatur | Tidak disebut | Tidak ada SLA freshness | 1 |
| US-4. Data contract dengan pemilik sumber | Kontrak skema, semantik, kualitas minimum, dan notifikasi perubahan dengan owner aplikasi sumber | PDM wajib sesuai Kamus Data & EDM (D1 hlm. 16–17) dapat menjadi dasar | Kamus Data antar-skema tidak konsisten (D2 hlm. 26). Kewenangan perubahan elemen data tidak jelas (D2 hlm. 26) | Tidak ada kontrak; perubahan skema sumber berisiko memutus pipeline | 1 |
| US-5. Beban minimum pada sumber | Ingest dari standby/log, bukan query ke database produksi | Tidak diatur | ADG standby dipakai sebagai staging (D2 hlm. 21); CDC Qlik Replicate (D2 hlm. 21) | Praktik baik ada, tetapi tidak menjadi prinsip tertulis dan tidak diukur | 2 |
| US-6. Ingest sumber eksternal | Data eksternal masuk melalui jalur terkontrol, dengan validasi format dan asal | Ps. 66 untuk pertukaran eksternal (D1 hlm. 50–51); skema verifikasi dan pertukaran data (D1 hlm. 54) | Sistem sudah mengakomodasi data eksternal; risiko format berbeda (D2 hlm. 21) | Jalur, validasi, dan pencatatan asal data eksternal tidak terdokumentasi | 2 |
| US-7. Pemantauan, rekonsiliasi & lineage ingest | Monitoring per pipeline, rekonsiliasi jumlah/nilai, lineage sumber → platform | Integrasi DWBI dengan metadata (D1 hlm. 76) | Dua katalog terpisah: Dataedo dan Atlas (D2 hlm. 26). Monitoring belum dipetakan ke aplikasi (D2 hlm. 50) | Tidak ada rekonsiliasi atau lineage end-to-end | 1 |

**Rata-rata upstream: ±1,6.**

### 3.2 Downstream

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| DS-1. Konsumsi internal (BI & dashboard) | Self-service terkontrol di atas data mart terkurasi | Prinsip self-service dan transparansi DWBI (D1 hlm. 76) | 17 dashboard di Tableau/QlikSense; analisis via request ke MDT, lambat (D2 hlm. 22) | Self-service belum ada | 2 |
| DS-2. Distribusi ke aplikasi internal (skor, insight) | Hasil analitik dikirim ke aplikasi operasional via API/event dengan kontrak | Tidak diatur khusus | Tidak terdokumentasi | Jalur balik platform → aplikasi tidak terlihat | 1 [perlu konfirmasi] |
| DS-3. Distribusi ke mitra K/L (API, SFTP) | Satu pintu keluar, pola seragam, terkait PKS | Ps. 66(3): web service dan SFTP melalui host-to-host (D1 hlm. 51) | API untuk pertukaran eksternal; SFTP bulk untuk PKS K/L (D2 hlm. 21). Integrasi eksternal per aplikasi, belum terpusat (D2 hlm. 40) | Belum ada pintu tunggal; asal data (platform atau aplikasi) tidak jelas | 2 |
| DS-4. Data sebagai layanan | Katalog produk data, proses permintaan, value data | Skema data sebagai layanan (D1 hlm. 54, Ps. 69(3)); value data (D1 hlm. 56–57, Ps. 72) | Data Collaboration memakai platform eksternal beragam (D2 hlm. 22) | Belum ada katalog produk data atau platform baku | 2 |
| DS-5. Standar API & data contract konsumen | Standar desain (REST, versioning, error, paging), spesifikasi terbuka, kontrak per konsumen | Tidak diatur | Tidak disebut | Tidak ada standar maupun kontrak | 1 |
| DS-6. API gateway & kontrol akses berbasis klasifikasi | Gateway tunggal; autentikasi, otorisasi, rate limit; kontrol mengikuti klasifikasi Biasa/Terbatas/Rahasia | Ps. 66(5): otorisasi, otentikasi, akses kontrol, perimeter, IP whitelist (D1 hlm. 51). Data Rahasia wajib pertukaran terenkripsi (D1 hlm. 93–94) | API Security, API Management (D2 hlm. 37); API Gateway (D2 hlm. 50); tidak tampak di diagram integrasi (D2 hlm. 39–40). Risiko kebocoran pengiriman eksternal diakui (D2 hlm. 21–22) | Status gateway dan penerapan kontrol per klasifikasi tidak terbukti | 2 [perlu konfirmasi] |
| DS-7. SLA & pemantauan konsumsi | SLA ketersediaan/latensi per layanan, analitik pemakaian, alerting | Monev kinerja TI umum (D1 hlm. 58, Ps. 75) | Tidak disebut | Tidak ada SLA layanan data | 1 |
| DS-8. Tata kelola PKS & onboarding mitra | PKS memuat metode integrasi, elemen data, keamanan; onboarding teknis baku | Isi minimum PKS (D1 hlm. 56, Ps. 71); kriteria mitra Non-KL termasuk teknologi integrasi (D1 hlm. 55); mekanisme PKS (D1 hlm. 56–58, Ps. 73) | SFTP bulk berbasis PKS K/L (D2 hlm. 21) | Daftar PKS aktif dan keterkaitannya dengan interface tidak terdokumentasi | 2 |

**Rata-rata downstream: ±1,6.**

### 3.3 Catatan: integrasi operasional (tidak dinilai sebagai lingkup inti)
- Internal: pola beragam, Direct-to-DB masih ada, SMILE sebagai hub berpotensi SPOF (D2 hlm. 39). Setara skor 2 bila dinilai.
- Eksternal: H2H dan API, belum seragam, per aplikasi (D2 hlm. 40). Setara skor 2 bila dinilai.
- Lingkup SMILE lebih luas daripada distribusi hasil platform data. Untuk kajian ini, SMILE diperlakukan sebagai **sumber utama upstream** dan **pengguna standar** yang sama, bukan objek desain.

### 3.4 Tidak dinilai
- Kewajiban integrasi pemerintah (Satu Data Indonesia, D1 hlm. 53 Ps. 68; Inpres 4/2025 sebagai dasar, D1 hlm. 3; integrasi Taspen/Asabri 2029): mekanisme tidak disebut di D2. Belum ada bukti.

## 4. Temuan utama
1. **Upstream sudah layak dijadikan fondasi.** ADG, Talend, dan Qlik Replicate berjalan (D2 hlm. 21), dan ingest dari standby/CDC sudah membatasi beban pada sumber produksi. Yang belum ada adalah lapisan pengelolaan: daftar sumber, SLA freshness, kontrak dengan pemilik sumber, dan rekonsiliasi.
2. **Downstream belum dirancang sebagai layanan.** Konsumsi internal lewat request ke MDT (D2 hlm. 22), distribusi eksternal per aplikasi (D2 hlm. 40), dan Data Collaboration lewat platform yang beragam (D2 hlm. 22). Hasil analitik tidak terlihat kembali ke aplikasi operasional.
3. **Kebijakan mengatur "boleh atau tidak", bukan "bagaimana".** Ps. 66 dan Ps. 69–73 kuat untuk otorisasi dan PKS (D1 hlm. 50–58), tetapi tidak ada standar API, data contract, SLA, atau pemantauan interface. Ps. 66(3) belum menyebut API gateway, OAuth2/mTLS, atau event streaming, sehingga pola tersebut bergantung pada kajian kasus per kasus Ps. 66(4) (D1 hlm. 51).
4. **Perubahan di sumber adalah risiko upstream terbesar.** Kewenangan perubahan elemen data tidak jelas dan Kamus Data tidak konsisten (D2 hlm. 26). Tanpa kontrak, perubahan skema SMILE dapat memutus pipeline tanpa peringatan.
5. **Integrasi operasional tetap menentukan keberhasilan standar.** Direct-to-DB dan peran SMILE sebagai hub (D2 hlm. 39) berada di luar lingkup inti, tetapi standar yang tidak berlaku bagi SMILE akan melahirkan dua cara kerja.
6. **Lanskap eksternal di D2 kemungkinan tidak lengkap.** DJP, BPJS Kesehatan (e-PLKK), dan Kemnaker SIAPkerja diketahui dari riset publik, tetapi koneksi ke DJP dan BPJS Kesehatan tidak tergambar di D2 hlm. 40 [perlu konfirmasi].

## 5. Rekomendasi (untuk didiskusikan)

### 5.1 Prinsip integrasi terstandar (berlaku untuk upstream, downstream, dan integrasi operasional)
1. **Tidak ada Direct-to-DB lintas aplikasi.** Akses melalui API, CDC, atau layanan integrasi ESDP; penghentian bertahap, dimulai dari integrasi ke SMILE (D2 hlm. 39).
2. **Setiap interface punya pemilik dan data contract** (skema, semantik, kualitas minimum, SLA, versi), diturunkan dari Kamus Data & EDM (D1 hlm. 16–17) dan disimpan di katalog metadata (D2 hlm. 26).
3. **Kontrol keamanan mengikuti klasifikasi data** (D1 hlm. 93–94) dan Ps. 66 (D1 hlm. 50–51).
4. **Satu pintu untuk pertukaran eksternal** melalui gateway terkelola.
5. **Setiap interface terpantau** (latensi, volume, kegagalan) dan **direkonsiliasi**.
6. **Pola dipilih berdasarkan kebutuhan**, bukan kebiasaan: CDC/event untuk perubahan, API untuk permintaan sinkron, file/SFTP untuk bulk.

Prinsip ini disusun agar dapat diadopsi sebagai juknis turunan atau masukan revisi Ps. 66.

### 5.2 Upstream

| Fokus | Rekomendasi | Basis eksisting |
|---|---|---|
| Kelengkapan | Daftar sumber prioritas per Data Building Block, dengan status ingest dan ukuran kelengkapan | DBB (D2 hlm. 16–18) |
| Pola | CDC-first untuk sumber transaksional; batch/ETL untuk sumber tanpa log atau referensi; SFTP terkelola (manifest, checksum) untuk sumber eksternal bulk | Qlik Replicate, Talend, ADG, SFTP (D2 hlm. 21) |
| Freshness | SLA freshness per domain (mis. near real-time untuk kepesertaan, harian untuk keuangan) [perlu konfirmasi kebutuhan bisnis] | – |
| Data contract | Kontrak dengan owner aplikasi sumber, termasuk notifikasi perubahan skema sebelum rilis | Kamus Data, EDM (D1 hlm. 16–17) |
| Beban sumber | Prinsip tertulis: ingest dari standby atau log, bukan query ke produksi | ADG standby (D2 hlm. 21) |
| Pemantauan | Rekonsiliasi jumlah/checksum per pipeline; lineage sumber → platform | Dataedo/Atlas (D2 hlm. 26); monitoring (D2 hlm. 50) |

### 5.3 Downstream

| Fokus | Rekomendasi | Basis eksisting |
|---|---|---|
| Pintu layanan | API gateway tunggal untuk konsumen internal dan eksternal; portal mitra | API Security/Management, API Gateway (D2 hlm. 37, 50) [perlu konfirmasi produk] |
| Standar API | Standar desain, versioning, error, paging; spesifikasi terbuka per layanan | – |
| Kontrol akses | Otorisasi berbasis klasifikasi dan tujuan PKS; enkripsi untuk data Rahasia; log akses | D1 hlm. 51, 93–94 |
| SLA | SLA per layanan data dan pemantauan pemakaian | – |
| PKS | Setiap layanan eksternal terhubung ke PKS (tujuan, metode integrasi, elemen data, Ps. 71) | D1 hlm. 56 |
| Konsumsi internal | Self-service terkontrol di atas data mart; jalur balik skor/insight ke aplikasi via API/event | Tableau, QlikSense (D2 hlm. 22); D1 hlm. 76 |
| Data sebagai layanan | Katalog produk data dan satu platform kolaborasi | D1 hlm. 54; D2 hlm. 22 |

### 5.4 Diagram target integrasi (usulan)

![[12-Integrasi-Upstream-Downstream.excalidraw]]

## 6. Implikasi ke Blueprint (batas peran per sisi)

| Komponen | Upstream | Downstream |
|---|---|---|
| SoR (SMILE dan aplikasi operasional) | Sumber data; pemilik skema dan pihak kontrak. Menyediakan CDC/standby, bukan akses langsung ke produksi | Konsumen skor/insight melalui API/event. Integrasi operasional SMILE ke aplikasi lain tetap di luar platform, tetapi mengikuti prinsip 5.1 |
| MDM | Mengambil data dari sumber untuk match/merge (lihat [[11-Master-Data]]) | Menyediakan golden record dan kode referensi ke platform dan aplikasi via API/event |
| ESDP (integrasi sebagai salah satu komponen) | Lapisan ingest: CDC, batch, SFTP terkelola, data contract, rekonsiliasi | Lapisan layanan: API gateway, katalog layanan/produk data, kontrol akses, SLA, pemantauan; pintu tunggal pertukaran eksternal. Definisi ESDP [perlu konfirmasi] |
| Cloudera (big data/DWH) | Tujuan ingest (Data Lake, DWH) | Sumber data mart, analitik, dan data sebagai layanan, disajikan melalui ESDP. Tidak diakses langsung oleh mitra eksternal |
| Metadata (Dataedo/Atlas) | Lineage ingest, definisi kontrak sumber | Spesifikasi layanan dan kontrak konsumen |

## 7. Pertanyaan terbuka untuk BPJS
**Upstream**
1. Sumber apa saja yang saat ini masuk ke platform, dengan pola apa (ADG, Talend, Qlik Replicate, SFTP), dan dengan frekuensi berapa?
2. Bagaimana database non-Oracle (Yugabyte, SQL Server, PostgreSQL) di-ingest, jika sudah?
3. Apakah data eksternal (Dukcapil, DJP, BPJS Kesehatan, Kemnaker) masuk ke platform atau hanya ke aplikasi operasional?
4. Bagaimana tim platform mengetahui perubahan skema di SMILE sebelum dirilis?
5. Apakah Data Staging (standby ADG) sama dengan ODS (D1 hlm. 77, tender Oracle ODS)?

**Downstream**
6. Siapa saja konsumen data platform (unit, aplikasi, mitra), dan melalui jalur apa?
7. Apakah ada aplikasi operasional yang menerima skor atau insight dari platform?
8. Pertukaran ke mitra K/L (API/SFTP) diambil dari platform atau dari aplikasi operasional?
9. Produk apa yang dipakai untuk API Management dan API Gateway (D2 hlm. 37, 50), dan apakah sudah operasional?
10. Platform eksternal apa yang dipakai untuk Data Collaboration (D2 hlm. 22)?

**Lintas sisi**
11. Daftar interface (internal, eksternal, platform) beserta owner, frekuensi, volume, dan format.
12. Teknologi mana yang sudah melalui kajian keamanan Ps. 66(4)?
13. Apa definisi dan cakupan ESDP menurut BPJS TK?

## 8. Poin diskusi (ikpu)
- [ ] Validasi skor per sisi, terutama US-2 (calon 3) dan DS-6 (bergantung konfirmasi produk gateway).
- [ ] Sepakati apakah DS-2 (distribusi ke aplikasi internal) masuk use case 7.2 atau menjadi inisiatif roadmap tahun 2–3.
- [ ] Putuskan bentuk keluaran prinsip integrasi: lampiran Blueprint, juknis, atau draf usulan revisi Ps. 66.
- [ ] Tetapkan apakah pertukaran eksternal yang kini langsung dari aplikasi operasional (D2 hlm. 40) wajib dipindah ke pintu ESDP, atau cukup memenuhi standar yang sama.
- [ ] Tentukan kebutuhan SLA freshness per domain bersama unit bisnis sebelum desain pola upstream.
- [ ] Prioritaskan pertanyaan 1, 3, 8, 9, dan 11 untuk sesi dengan Asdep Interoperability & Processing (D2 hlm. 51).

## Keputusan Diskusi (26 Okt 2026)
1. **Standar pertukaran data:** Blueprint merekomendasikan pendekatan terstandar (standar API, keamanan, event, data contract) yang nantinya dapat menjadi dasar revisi Perdir (Ps. 66).
2. **Posisi integrasi:** Integrasi adalah salah satu komponen ESDP.
3. **Inventaris interface & Direct-to-DB:** Daftar interface diajukan sebagai pertanyaan langsung ke stakeholder (lihat [[12-Integrasi-Klarifikasi]]). Dari sisi kajian, Blueprint memperkenalkan **prinsip integrasi** sebagai pendekatan standar, termasuk penghentian bertahap akses Direct-to-DB dan peralihan ke API atau layanan integrasi ESDP.
4. **Kerangka penilaian:** Platform data (ESDP) menjadi poros. Integrasi dinilai dari dua sisi: **upstream** (sumber → platform; fokus kelengkapan, freshness, data contract dengan pemilik sumber, beban minimum pada sumber) dan **downstream** (platform → pengguna data; fokus standar API, kontrol akses & klasifikasi, SLA, PKS). Skor dihitung terpisah per sisi.
5. **Integrasi operasional:** Integrasi antar-aplikasi (SMILE ke aplikasi lain, H2H di luar platform) di luar lingkup inti, tetapi standar dan prinsip yang sama wajib berlaku. Lingkup SMILE lebih luas daripada distribusi hasil platform data; hal ini dapat diterima selama standar tersebut dapat dipakai untuk SMILE.

### Keputusan Diskusi lanjutan (26 Okt 2026)
6. **Istilah:** gunakan "freshness", bukan "kesegaran".
7. **Distribusi hasil analitik ke aplikasi internal (DS-2):** ditempatkan sesuai tema besar roadmap. Roadmap disusun dengan **tema besar per tahun/semester** sebagai milestone; DS-2 kandidat tema tahap lanjut (pemanfaatan analitik ke operasional).
8. **Letak prinsip integrasi:** bagian dari Blueprint; usulan awal di Bab Prinsip Arsitektur (subbab Prinsip Integrasi), detail standar di kerangka interoperabilitas [perlu dikonfirmasi ikpu].
9. **Pintu ESDP:** secara desain pertukaran eksternal diarahkan lewat ESDP, tetapi integrasi langsung tetap dimungkinkan bila lebih efisien **dan** memenuhi standar.
10. **SLA & parameter layanan:** belum perlu ditetapkan nilainya; Blueprint cukup mendefinisikan **daftar parameter yang wajib didefinisikan** per interface/domain (mis. SLA freshness, ketersediaan, latensi, volume, retensi, owner).
