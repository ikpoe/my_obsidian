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
1. Entitas apa saja yang secara resmi dikategorikan master data dan reference data (mis. Peserta/Tenaga Kerja, Pemberi Kerja/Badan Usaha, Kantor Cabang, Wilayah, Program, Mitra)? *(Sebagian terjawab, 31 Okt 2026: model konseptual Kepesertaan memuat 13 entitas berdefinisi (D3-05); status resmi kategori master/referensi belum dikonfirmasi)*
2. Apakah sudah ada hasil analisis lingkup master data dan reference data sebagaimana diamanatkan Pedoman Tata Kelola Data (hlm. 87–88)?
3. Bagaimana tafsiran internal atas "repository master data tunggal terpusat": hub fisik (sistem MDM), atau tata kelola terpusat atas data yang tersebar?

## B. Sumber Kebenaran & Identitas
4. Untuk setiap entitas, aplikasi apa yang menjadi sumber kebenaran (system of record)? Apakah SMILE menjadi satu-satunya sumber untuk data Peserta dan Pemberi Kerja?
6. Apakah ada data peserta yang belum ber-NIK, NIK ganda, atau NIK tidak valid? Jika ada, berapa perkiraan proporsinya?
7. Bagaimana mekanisme verifikasi NIK ke Dukcapil (real-time saat registrasi, batch berkala, atau keduanya)? Apakah hasil verifikasi disimpan kembali ke data peserta?

## C. Kepemilikan & Proses
8. Siapa pemilik data (data owner) dan pengelola data (data steward) untuk setiap entitas master saat ini? Siapa yang berperan sebagai *data liaison*?
9. Bagaimana alur penambahan dan perubahan master data saat ini (siapa mengusulkan, menyetujui, dan mengeksekusi)? Apakah sudah ada SOP, walau belum formal?

## D. Distribusi & Pemanfaatan
12. Paparan EA menyebut big data belum merujuk ke master data. Bagaimana kondisinya saat ini, dan apakah ada inisiatif perbaikan? *(Sebagian terjawab, 31 Okt 2026: POC MDM menghasilkan golden record ke tabel data mart (D3-02), belum produksi)*

## E. Reference Data
13. Bagaimana tabel kode referensi dikelola (mis. kode wilayah, jenis pekerjaan, sektor usaha, kode program)? Apakah tersebar per aplikasi atau terpusat?
14. Apakah ada standar eksternal yang wajib diikuti (mis. kode wilayah Kemendagri, KBLI, KBJI)?

## F. Rencana & Inisiatif
16. Apa definisi dan cakupan ESDP menurut BPJS TK, dan bagaimana posisinya terhadap master data?
17. Terkait integrasi Taspen dan Asabri, apakah sudah ada kajian awal mengenai penyatuan data peserta (identitas, kunci, dan duplikasi lintas lembaga)?

## G. Permintaan Data / Dokumen Pendukung
- [ ] Daftar entitas master beserta aplikasi sumbernya
- [ ] Contoh struktur tabel peserta dan pemberi kerja (tanpa data pribadi)
- [ ] Statistik kualitas data identitas (NIK kosong/ganda/tidak valid), jika tersedia
- [ ] Dokumen SOP atau alur perubahan data, jika ada
- [ ] Daftar tabel kode referensi dan pengelolanya
- [ ] Dokumen rencana strategis TI / ITMP terkait data

## Terjawab (pembaruan 31 Okt 2026)
> Pertanyaan berikut dipindahkan dari daftar di atas karena sudah dijawab. Nomor asli dipertahankan sebagai rujukan.

| No. asli | Pertanyaan | Status | Jawaban & sumber |
|---|---|---|---|
| 5 | Kunci identitas apa yang dipakai untuk Peserta (NIK, nomor KPJ, lainnya) dan untuk Pemberi Kerja (NPP, NPWP, NIB)? | **Terjawab** | Peserta: KPJ (dikenal peserta), kode TK (ID internal), kode kepesertaan = kode perusahaan + kode TK; Pemberi Kerja: NPP (kode perusahaan). Sistem dibangun sendiri (R1 [P 00:34:11–00:35:13]). NIB dipakai pada API Kemnaker (D5 hlm. 11–12). |
| 10 | Bagaimana penanganan data ganda (deduplikasi/penggabungan) dan siapa yang berwenang memutuskannya? | **Terjawab** | Tidak ada mekanisme baku; pembina cabang berkoordinasi ke kantor pusat lewat rapat terbatas, TK dipindah ke kode perusahaan yang bertahan, kasus per kasus (R1 [P 01:06:06–01:08:42]). |
| 11 | Bagaimana master data saat ini didistribusikan ke aplikasi lain dan ke platform big data (replikasi DB, CDC Qlik Replicate, ETL Talend, API, atau salin manual)? | **Terjawab** | Ke big data: Qlik Replicate CDC dari log Oracle, termasuk initial load, paling cepat per jam; ETL dengan Talend (R1 [S 00:01:56–00:08:24]). Ke mitra: API melalui API Management dan kueri DB link (D5 hlm. 1–5). |
| 15 | Apakah ada rencana pengadaan atau inisiatif MDM / reference data management dalam RKAT atau rencana strategis TI 2025–2027? | **Terjawab** | MDM dibangun internal oleh tim BPJS, tahap POC (D3-01, D3-02; R1 [P 01:25:45–01:26:50]). Bukan pengadaan. |

## Pertanyaan baru (31 Okt 2026)
18. **Status POC MDM terhadap target Okt 2026:** apakah POC selesai sesuai target Okt 2026? Apa hasil, cakupan entitas, dan keputusan lanjutannya (produksi, perluasan, atau penundaan)?
19. **Isi golden record:** apakah saldo dan atribut transaksi lain memang dimaksudkan masuk golden record (contoh D3-02), atau hanya ilustrasi? (lihat [[11-Master-Data]] §4A)
20. **Aturan pembobotan:** bagaimana bobot per sumber (JMO, POM, Lapak Asik, SMILE) ditentukan dan siapa yang menyetujuinya?
21. **Data Hub:** apakah Data Hub/penampung golden record yang dapat diakses dari luar akan dibangun, oleh siapa, dan kapan?
22. **Konflik status DRC:** D3-01 menyebut DRC aktif dengan 20 node dan MDM Engine, sedangkan R1 [S 00:35:03–00:37:39] menyebut belum ada komponen Cloudera di DRC. Mana yang berlaku, dan di mana mesin MDM berjalan?
23. **Pembentukan kode TK dan kode kepesertaan:** di sistem mana kode di-generate dan apa aturannya (R1 [P 00:35:24–00:35:52])?
24. **BPU dan "wadah":** bagaimana wadah BPU dimodelkan terhadap Pemberi Kerja/Unit Pemberi Kerja (R1 [P 00:36:25–00:36:44])?
