---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Integrasi & Interoperabilitas Data
sesi: 2
versi: 2 (struktur upstream–downstream, 26 Okt 2026)
status: siap-dikirim
tags: [klarifikasi, integrasi, interoperabilitas]
---
# Daftar Klarifikasi — Integrasi & Interoperabilitas Data

> Konteks: penilaian awal hanya berbasis dokumen Pedoman Tata Kelola Data dan TI (Perdir 4/2025) dan paparan Enterprise Architecture As-Is. Integrasi dikaji dengan platform data (ESDP) sebagai poros: **upstream** (sumber data → platform) dan **downstream** (platform → pengguna data). Integrasi operasional antar-aplikasi dikaji sebatas penerapan standar. Klarifikasi berikut untuk memvalidasi kondisi aktual. Rujukan: [[12-Integrasi]].

## A. Inventaris Interface (lintas sisi)
1. Mohon daftar interface yang aktif saat ini, baik dari sumber ke platform data, dari platform ke pengguna, maupun antar-aplikasi, beserta sumber, tujuan, pola, frekuensi, volume, format, dan pemiliknya. Jika belum ada daftar formal, perkiraan jumlah per kategori sudah membantu.
2. Unit mana yang memiliki interface: Asdep Interoperability & Processing, unit pengembang aplikasi, atau unit bisnis pemrakarsa?

## B. Upstream — Sumber Data ke Platform
3. Aplikasi/database apa saja yang saat ini masuk ke platform data, dan dengan pola apa (Active Data Guard, Talend, Qlik Replicate, SFTP)? Berapa frekuensinya?
4. Bagaimana database selain Oracle (MariaDB, Yugabyte, SQL Server, PostgreSQL) di-ingest ke platform, jika sudah?
5. Apakah ada target freshness data (mis. near real-time, harian) per domain, dan apakah pencapaiannya dipantau?
6. Bagaimana tim platform mengetahui perubahan struktur data di SMILE atau aplikasi sumber lain sebelum dirilis?
7. Apakah ada rekonsiliasi (jumlah record, nilai kontrol) antara sumber dan platform?
8. Apakah data dari pihak eksternal (Dukcapil, DJP, Kemnaker SIAPkerja, BPJS Kesehatan) masuk ke platform data, atau hanya ke aplikasi operasional? Bagaimana format dan validasinya?
9. Apakah Data Staging (standby database berbasis Active Data Guard) juga berfungsi sebagai Operational Data Store?

## C. Downstream — Platform ke Pengguna Data
10. Siapa saja pengguna data platform saat ini (unit kerja, aplikasi, mitra eksternal), dan melalui jalur apa (dashboard, ekspor, API, SFTP)?
11. Apakah ada aplikasi operasional yang menerima hasil analitik (skor, segmentasi, rekomendasi) dari platform? Jika ada, melalui mekanisme apa?
12. Pertukaran data dengan mitra K/L melalui API atau SFTP: apakah datanya diambil dari platform data atau langsung dari aplikasi operasional?
13. Platform eksternal apa saja yang dipakai untuk Data Collaboration, dan layanan data sebagai layanan apa yang sudah berjalan?
14. Apakah ada SLA (ketersediaan, latensi) untuk layanan data ke pengguna internal maupun mitra?
15. Apakah ada rencana self-service analitik untuk unit bisnis, mengingat permintaan analisis saat ini melalui MDT?

## D. Integrasi Operasional (penerapan standar)
16. Integrasi antar-aplikasi mana saja yang masih menggunakan akses langsung ke database (Direct to DB)? Apakah ada rencana penggantiannya?
17. Produk apa yang digunakan sebagai Middleware/Web Service? Apakah ada Enterprise Service Bus atau platform integrasi terpusat?
18. Apakah ada rencana pengurangan ketergantungan pada SMILE sebagai hub integrasi?

## E. Keamanan, Standar & Kepatuhan
19. Produk apa yang digunakan untuk API Management dan API Gateway? Apakah sudah operasional dan dipakai oleh seluruh API? Apa cakupan pengadaan keamanan API tahun 2024?
20. Bagaimana proses persetujuan akses interoperabilitas eksternal sesuai Pasal 66 dijalankan dan dicatat?
21. Teknologi selain web service dan SFTP apa saja yang sudah melalui kajian keamanan Pasal 66 ayat (4)? Apakah hasilnya terdokumentasi?
22. Mekanisme autentikasi apa yang dipakai untuk API eksternal (API key, OAuth2, mTLS, IP whitelist)? Apakah kontrol dibedakan menurut klasifikasi data (Biasa, Terbatas, Rahasia)?
23. Apakah ada standar desain API (penamaan, versioning, format error) atau template spesifikasi interface?
24. Apakah spesifikasi data (elemen, format, aturan validasi) dicantumkan dalam PKS atau dokumen teknis terpisah untuk setiap mitra?

## F. Rencana & Inisiatif
25. Apa definisi dan cakupan ESDP menurut BPJS TK, dan apakah ESDP diharapkan menjadi pintu tunggal pertukaran data eksternal?
26. Bagaimana rencana integrasi terkait Satu Data Indonesia, DTSEN (Inpres 4/2025), dan integrasi Taspen/Asabri?
27. Apakah ada rencana event streaming / message broker?

## G. Permintaan Data / Dokumen Pendukung
- [ ] Daftar interface atau katalog API (upstream, downstream, operasional) beserta atribut yang tersedia
- [ ] Daftar job Talend dan task Qlik Replicate (ringkasan: sumber, tujuan, jadwal)
- [ ] Daftar konsumen data platform (unit, aplikasi, mitra) dan jalurnya
- [ ] Diagram integrasi terbaru dalam resolusi penuh (pengganti paparan EA hlm. 39–40)
- [ ] Spesifikasi teknis interface untuk 3–5 integrasi kritikal (Dukcapil, DJP, bank, BPJS Kesehatan)
- [ ] Daftar PKS pertukaran data aktif beserta metode integrasinya (tanpa informasi rahasia)
- [ ] Hasil kajian keamanan teknologi interoperabilitas (Pasal 66 ayat 4), jika ada
- [ ] Standar/pedoman pengembangan API, jika ada
- [ ] Laporan monitoring atau insiden integrasi 12 bulan terakhir, jika tersedia
- [ ] Dokumen rencana strategis TI / ITMP terkait integrasi dan modernisasi aplikasi
