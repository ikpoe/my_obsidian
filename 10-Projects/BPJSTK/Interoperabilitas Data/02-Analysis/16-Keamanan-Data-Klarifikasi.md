---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Keamanan Data
sesi: 6
status: siap-dikirim
tags: [klarifikasi, keamanan-data, pdp]
---
# Daftar Klarifikasi — Keamanan Data

> Konteks: penilaian awal hanya berbasis dokumen Pedoman Tata Kelola Data dan TI (Perdir 4/2025) dan paparan Enterprise Architecture As-Is. Klarifikasi berikut untuk memvalidasi kondisi aktual pengamanan data, terutama pada platform data dan pertukaran data. Rujukan: [[16-Keamanan-Data]].

## A. Tata Kelola & PDP
1. Apakah peraturan direksi tentang Pelindungan Data Pribadi (dirujuk Ps. 3(4)) sudah terbit? Mohon salinannya.
2. Apakah Pejabat Pelindungan Data Pribadi sudah ditunjuk, dan bagaimana perannya terhadap Unit Manajemen Data dan OKI?
3. Siapa yang bertanggung jawab atas keamanan data di platform data: Unit Manajemen Data, Asdep Keamanan TI, atau Asdep Platform Teknologi & Keamanan?

## B. Klasifikasi Data
4. Apakah klasifikasi Biasa/Terbatas/Rahasia sudah diterapkan per entitas/elemen data? Di mana dicatat (Dataedo, Atlas, dokumen)? *(Sebagian terjawab, 31 Okt 2026: kebijakan klasifikasi belum ada (R1 [P 01:36:45–01:38:45]))*
5. Apakah ada pembedaan data pribadi umum dan spesifik sesuai UU 27/2022?
6. Kapan klasifikasi terakhir ditinjau, dan siapa yang menyetujuinya?

## C. Kontrol Akses
7. Apakah matriks kontrol akses data (CRUD per elemen per peran) sudah disusun dan dipakai?
8. Apakah Apache Ranger (atau Sentry), Kerberos, dan integrasi LDAP/AD aktif di Cloudera?
9. Bagaimana hak akses ke data mart dan dashboard (Tableau, QlikSense) diberikan: akun servis bersama atau identitas pengguna?
10. Apakah reviu hak akses tahunan dan PAM sudah berjalan untuk platform data? Mohon contoh laporan terakhir.

## D. Masking, Tokenisasi & Enkripsi
11. Apakah ada masking atau tokenisasi untuk NIK, no. KPJ, rekening, atau data upah? *(Sebagian terjawab, 31 Okt 2026: masking sebagian (R1 [P 00:09:26–00:09:29]); respons API BPJS ter-masking (D5 hlm. 7–8); D3-01 Data Masking kuning; tokenisasi belum diketahui)*
12. Data apa yang dipakai di lingkungan pengembangan/pengujian: salinan produksi, data ter-masking, atau sintetis?
13. Apakah enkripsi at-rest aktif di HDFS/Ozone dan database sumber? Di mana key enkripsi dikelola?

## E. Audit & Monitoring
14. Apakah akses baca ke data sensitif di platform tercatat (siapa, kapan, apa)? Ke mana log dikirim (SIEM)?
15. Siapa yang meninjau log akses data, dan seberapa sering?

## F. Pertukaran Eksternal
16. Berapa mitra yang terhubung via API, SFTP, dan akses database langsung? Mohon daftar per mitra (Dukcapil, DJP, Kemnaker, BPJS Kesehatan, perbankan). *(Sebagian terjawab, 31 Okt 2026: mitra rutin: Kemenkes, BPJS Kesehatan (R1 [P 01:29:29–01:29:43]), Kemnaker via API dengan DB link (D5); daftar lengkap belum ada)*
18. Apa lingkup tender API security 2024, dan apakah mencakup layanan pertukaran data?
19. Pernahkah terjadi insiden keamanan terkait pertukaran data, dan bagaimana penanganannya?

## G. Segregasi Platform
20. Apakah data lake dan DWH dipisahkan secara logis (namespace, cluster, kebijakan) meskipun satu environment?

## Terjawab (pembaruan 31 Okt 2026)
> Pertanyaan berikut dipindahkan dari daftar di atas karena sudah dijawab. Nomor asli dipertahankan sebagai rujukan.

| No. asli | Pertanyaan | Status | Jawaban & sumber |
|---|---|---|---|
| 17 | Bagaimana otentikasi API eksternal (API key, OAuth, mTLS) dan apakah file SFTP dienkripsi? | **Terjawab** | API Kemnaker: key validation di API Management dan token (D5 hlm. 1); SFTP JKP dienkripsi AES-256 (R1 [P 01:41:27–01:43:49]). |

## Pertanyaan baru (31 Okt 2026)
21. **Pengelolaan kredensial layanan pertukaran:** dokumen teknis API Kemnaker memuat kredensial dalam teks biasa. Apakah kredensial itu masih aktif, siapa yang memegang dokumen, dan apakah ada rencana memindahkannya ke secret manager serta merotasinya?
22. **Retensi data pribadi mitra:** berapa lama data dari Kemnaker yang disalin ke tabel lokal disimpan, siapa yang boleh mengakses, dan apa dasar pemrosesannya?
23. **Audit trail:** D3-01 menandai Audit Trail merah. Apakah akses ke API, DB link, dan tabel salinan mitra tercatat? Apa rencana perbaikannya?
24. **Kebijakan retensi:** dengan retensi data 40+ tahun (R1), apakah ada rencana jadwal retensi/arsip per kelas data?
25. **Cakupan 3scale untuk kontrol keamanan:** apakah semua mitra eksternal sudah melalui 3scale (D4), mengingat D3-01 menandai API Management merah?
