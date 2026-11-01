---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Data Warehouse & Business Intelligence
sesi: 9
status: siap-dikirim
tags: [klarifikasi, dwh, bi, kpi, semantik, esdp]
---
# Daftar Klarifikasi — Data Warehouse & Business Intelligence

> Konteks: lingkup hanya DW dan BI yang mengonsumsi platform data (ESDP/Cloudera) serta jalur downstream ke Data Hub. Laporan operasional aplikasi sumber tidak dinilai. Prioritas: **P1** = menentukan rekomendasi; **P2** = menentukan desain; **P3** = melengkapi skor. Rujukan: [[19-DW-BI]].

## A. Portofolio & Alat BI (P1)
1. Berapa jumlah dashboard, laporan, dan cube yang membaca data dari Cloudera? Mohon inventaris beserta alat (Tableau/QlikSense/cube), pemilik, dan jumlah pengguna.
2. Mengapa dipakai dua alat BI? Apakah ada pembagian per kelompok pengguna atau rencana konsolidasi?
3. Alat apa yang dipakai untuk cube finance, dan apakah cube tersebut membaca dari Cloudera?

## B. KPI & Lapisan Semantik (P1)
4. Apakah ada definisi KPI baku (mis. peserta aktif, iuran terkumpul, klaim terbayar) yang dipakai lintas dashboard? Siapa yang menetapkan dan mengubahnya?
5. Pernahkah dua dashboard menampilkan angka berbeda untuk KPI yang sama? Bagaimana diselesaikan?
6. Di mana logika perhitungan metrik saat ini berada: di job Talend, di view Cloudera, atau di model Tableau/QlikSense?

## C. Model Operasi & Permintaan (P1)
7. Bagaimana alur permintaan analitik dari unit bisnis ke tim data saat ini, dan berapa lama rata-rata penyelesaiannya?
8. Apakah unit bisnis ingin membuat dashboard sendiri? Unit mana yang paling siap untuk pilot self-service?
9. Berapa kapasitas tim BI/data mart saat ini?

## D. Model Gold & Dimensi (P2)
10. Apakah ada bus matrix atau daftar fact/dimensi per data mart? Dimensi Program dan Kanal saat ini disimpan di mana?
11. Tipe SCD apa yang dipakai di data mart (pertanyaan dari diskusi 27 Okt belum terjawab jelas)?

## E. Freshness & Akses (P2)
12. Laporan mana yang paling sensitif terhadap keterlambatan data, dan berapa toleransi yang diharapkan pengguna?
13. Bagaimana izin di Tableau/QlikSense diberikan? Apakah ada row-level security atau pembatasan ekspor untuk data pribadi?

## F. Downstream & Analitik Lanjut (P3)
14. Aplikasi atau core system mana yang membaca langsung dari environment big data, dan untuk apa?
15. Use case predictive apa yang sudah berjalan dengan Python/KNIME, dan di mana hasil model disimpan?
16. Apakah ada produk data gold yang direncanakan untuk dibagikan ke mitra melalui Data Hub?
