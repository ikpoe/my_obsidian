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
1. Apakah Data Building Block dan Enterprise Data Model telah diperbarui setelah IT Master Plan 2019–2024?
2. Apakah tersedia EDM lengkap per domain beserta model logisnya? Paparan EA hanya memuat sebagian halaman.
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
10. Pola model apa yang dipakai di Data Warehouse/EDW saat ini (mis. star schema, ternormalisasi, lainnya)?
11. Bagaimana data historis dan perubahan atribut (mis. perubahan status peserta atau alamat) disimpan?
12. Apakah zona data (raw, terintegrasi, siap pakai) sudah dipisahkan secara model maupun akses?
13. Format tabel dan engine apa yang dipakai di Cloudera (mis. Hive, Impala, Iceberg, Kudu)?
14. Apakah data mart untuk 17 dashboard berbagi dimensi yang sama, atau dibangun terpisah per dashboard?

## E. Master Data & Kunci
15. Kunci identitas apa yang dipakai secara resmi untuk peserta, tenaga kerja, dan pemberi kerja (mis. NIK, KPJ, NPP)?
16. Apakah dimensi di DWH sudah mengacu pada master data atau kode referensi yang sama?

## F. Tata Kelola & Kapasitas
17. Siapa yang saat ini berwenang menyetujui perubahan model data di platform?
18. Apakah model dan lineage terdokumentasi di Dataedo atau Apache Atlas?
19. Berapa jumlah dan kompetensi tim data engineer/modeler, dan apakah ada tool pemodelan atau otomasi pembangkitan model?
20. Apakah ada rencana atau kebutuhan audit (internal, BPK, OJK) yang mensyaratkan jejak historis data analitik?
