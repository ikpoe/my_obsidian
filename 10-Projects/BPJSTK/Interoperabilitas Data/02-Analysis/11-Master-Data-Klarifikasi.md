---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Master & Reference Data
sesi: 1
status: siap-dikirim
tags: [klarifikasi, master-data]
---
# Daftar Klarifikasi — Master & Reference Data

> Konteks: penilaian awal hanya berbasis dokumen Pedoman Tata Kelola Data dan TI (Perdir 4/2025) dan paparan Enterprise Architecture As-Is. Klarifikasi berikut untuk memvalidasi kondisi aktual. Rujukan: [[11-Master-Data]].

## A. Lingkup & Definisi
1. Entitas apa saja yang secara resmi dikategorikan master data dan reference data (mis. Peserta/Tenaga Kerja, Pemberi Kerja/Badan Usaha, Kantor Cabang, Wilayah, Program, Mitra)?
2. Apakah sudah ada hasil analisis lingkup master data dan reference data sebagaimana diamanatkan Pedoman Tata Kelola Data (hlm. 87–88)?
3. Bagaimana tafsiran internal atas "repository master data tunggal terpusat": hub fisik (sistem MDM), atau tata kelola terpusat atas data yang tersebar?

## B. Sumber Kebenaran & Identitas
4. Untuk setiap entitas, aplikasi apa yang menjadi sumber kebenaran (system of record)? Apakah SMILE menjadi satu-satunya sumber untuk data Peserta dan Pemberi Kerja?
5. Kunci identitas apa yang dipakai untuk Peserta (NIK, nomor KPJ, lainnya) dan untuk Pemberi Kerja (NPP, NPWP, NIB)?
6. Apakah ada data peserta yang belum ber-NIK, NIK ganda, atau NIK tidak valid? Jika ada, berapa perkiraan proporsinya?
7. Bagaimana mekanisme verifikasi NIK ke Dukcapil (real-time saat registrasi, batch berkala, atau keduanya)? Apakah hasil verifikasi disimpan kembali ke data peserta?

## C. Kepemilikan & Proses
8. Siapa pemilik data (data owner) dan pengelola data (data steward) untuk setiap entitas master saat ini? Siapa yang berperan sebagai *data liaison*?
9. Bagaimana alur penambahan dan perubahan master data saat ini (siapa mengusulkan, menyetujui, dan mengeksekusi)? Apakah sudah ada SOP, walau belum formal?
10. Bagaimana penanganan data ganda (deduplikasi/penggabungan) dan siapa yang berwenang memutuskannya?

## D. Distribusi & Pemanfaatan
11. Bagaimana master data saat ini didistribusikan ke aplikasi lain dan ke platform big data (replikasi DB, CDC Qlik Replicate, ETL Talend, API, atau salin manual)?
12. Paparan EA menyebut big data belum merujuk ke master data. Bagaimana kondisinya saat ini, dan apakah ada inisiatif perbaikan?

## E. Reference Data
13. Bagaimana tabel kode referensi dikelola (mis. kode wilayah, jenis pekerjaan, sektor usaha, kode program)? Apakah tersebar per aplikasi atau terpusat?
14. Apakah ada standar eksternal yang wajib diikuti (mis. kode wilayah Kemendagri, KBLI, KBJI)?

## F. Rencana & Inisiatif
15. Apakah ada rencana pengadaan atau inisiatif MDM / reference data management dalam RKAT atau rencana strategis TI 2025–2027?
16. Apa definisi dan cakupan ESDP menurut BPJS TK, dan bagaimana posisinya terhadap master data?
17. Terkait integrasi Taspen dan Asabri, apakah sudah ada kajian awal mengenai penyatuan data peserta (identitas, kunci, dan duplikasi lintas lembaga)?

## G. Permintaan Data / Dokumen Pendukung
- [ ] Daftar entitas master beserta aplikasi sumbernya
- [ ] Contoh struktur tabel peserta dan pemberi kerja (tanpa data pribadi)
- [ ] Statistik kualitas data identitas (NIK kosong/ganda/tidak valid), jika tersedia
- [ ] Dokumen SOP atau alur perubahan data, jika ada
- [ ] Daftar tabel kode referensi dan pengelolanya
- [ ] Dokumen rencana strategis TI / ITMP terkait data
