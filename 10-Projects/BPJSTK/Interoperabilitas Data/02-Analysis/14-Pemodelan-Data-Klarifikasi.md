---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Pemodelan & Desain Data
sesi: 4
status: siap-dikirim
tags: [klarifikasi, pemodelan-data]
---
# Daftar Klarifikasi — Pemodelan & Desain Data

> Konteks: penilaian awal hanya berbasis dokumen Pedoman Tata Kelola Data dan TI (Perdir 4/2025) dan paparan Enterprise Architecture As-Is. Klarifikasi berikut untuk memvalidasi kondisi aktual pemodelan data di aplikasi dan platform data (ESDP). Rujukan: [[14-Pemodelan-Data]].

## A. Model Enterprise & Konseptual
1. Apakah Data Building Block dan Enterprise Data Model telah diperbarui setelah IT Master Plan 2019–2024? *(Sebagian terjawab, 31 Okt 2026: model konseptual Kepesertaan dan Pelayanan tersedia (D3-04, D3-06); tanggal pembaruan belum diketahui dan JKP belum dimodelkan)*
2. Apakah tersedia EDM lengkap per domain beserta model logisnya? Paparan EA hanya memuat sebagian halaman. *(Sebagian terjawab, 31 Okt 2026: model konseptual dua domain tersedia (D3-04, D3-06); model logis belum terlihat)*
3. Entitas mana saja yang masih diolah dengan spreadsheet, dan apakah ada rencana untuk mensistemkannya?

## B. Model Fisik di Aplikasi
4. Apakah Physical Data Model untuk SMILE dan aplikasi lain tersedia dan terdokumentasi?
5. Apakah Unit Manajemen Data pernah mereviu kesesuaian PDM dengan EDM dan Kamus Data, sebagaimana diatur Pasal 14 ayat (5)?
6. Bagaimana perubahan skema pada SMILE atau aplikasi lain diinformasikan ke tim data?

## C. Kamus Data & Standar Penamaan
7. Sejauh mana Kamus Data telah terisi (jumlah elemen, domain yang tercakup), dan kapan terakhir diperbarui?
8. Apakah ada konvensi penamaan baku untuk database, skema, tabel, dan kolom di aplikasi maupun platform data?
9. Bahasa apa yang dipakai untuk nama teknis (Indonesia/Inggris), dan apakah sudah seragam?

## D. Model Platform Data
10. Pola model apa yang dipakai di Data Warehouse/EDW saat ini (mis. star schema, ternormalisasi, lainnya)? *(Sebagian terjawab, 31 Okt 2026: data mart di Kudu/Parquet (R1 [S 00:31:16–00:31:48]); pola (star/normal) belum disebut)*
14. Apakah data mart untuk 17 dashboard berbagi dimensi yang sama, atau dibangun terpisah per dashboard?

## E. Master Data & Kunci
16. Apakah dimensi di DWH sudah mengacu pada master data atau kode referensi yang sama?

## F. Tata Kelola & Kapasitas
17. Siapa yang saat ini berwenang menyetujui perubahan model data di platform?
19. Berapa jumlah dan kompetensi tim data engineer/modeler, dan apakah ada tool pemodelan atau otomasi pembangkitan model?
20. Apakah ada rencana atau kebutuhan audit (internal, BPK, OJK) yang mensyaratkan jejak historis data analitik?

## Terjawab (pembaruan 31 Okt 2026)
> Pertanyaan berikut dipindahkan dari daftar di atas karena sudah dijawab. Nomor asli dipertahankan sebagai rujukan.

| No. asli | Pertanyaan | Status | Jawaban & sumber |
|---|---|---|---|
| 11 | Bagaimana data historis dan perubahan atribut (mis. perubahan status peserta atau alamat) disimpan? | **Terjawab** | SCD sudah diterapkan (R1 [S 00:00:37–00:00:49]); model peserta lama mengulang data individu per program (R1 [P 01:21:33–01:22:26]). |
| 12 | Apakah zona data (raw, terintegrasi, siap pakai) sudah dipisahkan secara model maupun akses? | **Terjawab** | Belum; raw, model, dan data bersih hanya dibedakan skema dalam satu environment (R1 [S 00:44:54–00:45:06]). |
| 13 | Format tabel dan engine apa yang dipakai di Cloudera (mis. Hive, Impala, Iceberg, Kudu)? | **Terjawab** | Kudu untuk hasil CDC (update per jam), Parquet untuk analitik, HDFS; data mart Kudu/Parquet via Talend (R1 [S 00:17:55–00:19:21; S 00:29:19–00:31:48]). |
| 15 | Kunci identitas apa yang dipakai secara resmi untuk peserta, tenaga kerja, dan pemberi kerja (mis. NIK, KPJ, NPP)? | **Terjawab** | NIK untuk individu, KPJ/kode TK/kode kepesertaan untuk peserta, NPP untuk pemberi kerja; NIB untuk badan usaha di pertukaran dengan Kemnaker (R1 [P 00:34:11–00:35:13]; D5 hlm. 11–12). |
| 18 | Apakah model dan lineage terdokumentasi di Dataedo atau Apache Atlas? | **Terjawab** | Dataedo dipakai pasif (dokumentasi Oracle); lineage belum ada dan katalog belum dimanfaatkan (R1 [P 00:21:08–00:21:14; P 01:36:23–01:36:41]; D3-01 lineage & catalog merah). |

## Pertanyaan baru (31 Okt 2026)
21. **Kepesertaan dimensi atau fakta** (pertanyaan BPJS, R1 [P 01:32:39–01:33:45]): apakah BPJS setuju Peserta sebagai dimensi dan kepesertaan per pemberi kerja per periode sebagai fakta/snapshot?
22. **Grain dimensi Pemberi Kerja:** berapa NPP/UnitPemberiKerja yang dapat dimiliki satu Perusahaan, dan apakah laporan resmi disusun per NPP, per perusahaan, atau per grup?
23. **Individu vs Peserta:** apakah satu individu (NIK) dapat memiliki lebih dari satu kode TK aktif? Bagaimana kasus BPU yang dihimpun melalui wadah dimodelkan?
24. **Entitas Program dan Kanal:** apakah ada tabel referensi resmi untuk program dan kanal layanan? Siapa pemiliknya?
25. **Tipe SCD** yang dipakai per tabel (tipe 1/2) dan cakupannya.
26. **Model konseptual JKP:** kapan disusun?
