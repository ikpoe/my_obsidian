---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Penyimpanan Data + Dokumen & Konten
sesi: 8
status: siap-dikirim
tags: [klarifikasi, penyimpanan, dr, retensi, konten, esdp]
---
# Daftar Klarifikasi — Penyimpanan Data + Dokumen & Konten

> Konteks: lingkup hanya penyimpanan dan konten di platform data (ESDP/Cloudera) serta penyimpanan serving Data Hub. Database aplikasi sumber dan ECM/DMS tidak dinilai. Prioritas: **P1** = menentukan rekomendasi; **P2** = menentukan desain; **P3** = melengkapi skor. Rujukan: [[18-Penyimpanan-Dokumen-Konten]].

## A. DR & Backup Platform (P1)
1. Slide arsitektur current state menyebut "DRC (Active), 20 node", sedangkan diskusi 27 Okt menyebut komponen Cloudera di DRC belum ada (hanya rack dan uji coba satu mesin). Mana yang berlaku? Apakah 20 node itu klaster utama yang secara fisik berada di gedung DRC?
2. Apakah ada snapshot/backup HDFS dan Kudu, serta backup metadata klaster (metastore, Atlas, Ranger)? Ke mana disimpan?
3. Kapan restore klaster terakhir diuji, dan berapa lama prosesnya?
4. Apakah sudah ada target RPO/RTO untuk platform data atau untuk laporan/produk data tertentu?

## B. Kapasitas & Kinerja (P1)
5. Berapa kapasitas terpasang dan terpakai per zona (staging, lake, Kudu, Parquet, data mart)? Bagaimana tren pertumbuhannya?
6. Apakah ada dokumen perencanaan kapasitas (D1 Ps. 39) yang mencakup klaster Cloudera?
7. Apakah ada target freshness atau kinerja query tertulis? Laporan mana yang paling sensitif terhadap keterlambatan?

## C. Retensi, Arsip & Tiering di Platform (P2)
8. Berapa lama data disimpan di staging/landing dan di Kudu sebelum dipangkas? Apakah versi Parquet lama dihapus?
9. Apakah ada data di platform yang sudah diarsip atau dipindah ke storage lebih murah? Siapa yang memutuskan?
10. Apakah UMD sudah melakukan profiling kebutuhan archiving (D1 Lampiran II B) untuk tabel di platform?

## D. Dokumen & Konten di Platform (P2)
11. Jenis file apa yang saat ini masuk ke lake (Excel laporan iuran, scan, PDF, lampiran klaim)? Lewat jalur apa dan siapa pemiliknya?
12. Apakah ada kebutuhan analitik atas dokumen/lampiran klaim yang mengharuskan file disalin ke lake, atau cukup metadata + rujukan ke sistem dokumen?
13. Apakah file di lake sudah diberi klasifikasi atau label data pribadi?

## E. Pertukaran File via SFTP (P2)
14. Berapa lama file SFTP masuk dan keluar disimpan di server pertukaran, dan siapa yang menghapusnya?
15. Bagaimana validasi file saat ini (checksum, skema, jumlah baris)? Bagian mana yang masih manual?
16. Apakah PKS/dokumen teknis mengatur masa simpan dan pemusnahan file di sisi mitra?

## F. Serving Data Hub & Monitoring (P3)
17. Apakah sudah ada rancangan penyimpanan serving untuk Data Hub (cache, store terpisah di DMZ)?
18. Tool apa yang memantau klaster, job ETL, dan freshness? Apakah terhubung ke manajemen insiden?
