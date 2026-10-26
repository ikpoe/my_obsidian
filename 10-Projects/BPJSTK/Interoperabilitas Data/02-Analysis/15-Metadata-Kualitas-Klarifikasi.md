---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Metadata & Kualitas Data
sesi: 5
status: siap-dikirim
tags: [klarifikasi, metadata, kualitas-data]
---
# Daftar Klarifikasi — Metadata & Kualitas Data

> Konteks: penilaian awal hanya berbasis dokumen Pedoman Tata Kelola Data dan TI (Perdir 4/2025) dan paparan Enterprise Architecture As-Is. Klarifikasi berikut untuk memvalidasi kondisi aktual pengelolaan metadata dan kualitas data. Rujukan: [[15-Metadata-Kualitas]].

## A. Katalog & Metadata Teknis
1. Skema dan database apa saja yang sudah terdokumentasi di Dataedo, dan siapa penggunanya?
2. Apakah Apache Atlas aktif mencatat metadata dan lineage untuk seluruh job di Cloudera?
3. Apakah Dataedo dan Atlas sudah saling terhubung, atau dikelola terpisah?
4. Apakah lineage dari sumber (Talend, Qlik Replicate) sampai dashboard (Tableau, QlikSense) dapat ditelusuri?
5. Bagaimana perubahan struktur data (tabel/kolom baru) terdeteksi saat ini?

## B. Glossary & Kamus Data
6. Di mana Business Glossary dan Kamus Data disimpan, dan berapa istilah/elemen yang sudah tercakup?
7. Kapan terakhir dilakukan evaluasi tahunan glossary dan Kamus Data?
8. Siapa yang berwenang menyetujui perubahan definisi atau elemen data?

## C. Metadata Operasional
9. Apakah status job, volume, dan waktu pembaruan data dicatat dan dapat dilihat pengguna?
10. Apakah ada target freshness atau SLA ketersediaan data untuk dataset di platform?

## D. Kualitas Data
11. Tool kualitas data otomatis apa yang dipakai saat ini (mis. Talend Data Quality)?
12. Apakah daftar Critical Data Element, metrik, dan skala indeks kualitas sudah ditetapkan?
13. Domain apa yang sudah diprofiling, dan seberapa sering?
14. Apa isi Dashboard Kualitas Data, dan apakah laporan bulanan serta evaluasi triwulan berjalan?
15. Apakah ada pemeriksaan kualitas otomatis di pipeline sebelum data dipublikasikan?

## E. Isu & Perbaikan
16. Bagaimana alur pelaporan dan perbaikan bad data, terutama di kantor cabang? Apakah ada target waktu penyelesaian?
17. Berapa perkiraan backlog bad data saat ini, dan untuk domain apa?
18. Apakah ada validasi saat entri data di SMILE untuk elemen seperti NIK atau tanggal lahir?
19. Program awareness apa yang sudah dilakukan untuk petugas entri data?
