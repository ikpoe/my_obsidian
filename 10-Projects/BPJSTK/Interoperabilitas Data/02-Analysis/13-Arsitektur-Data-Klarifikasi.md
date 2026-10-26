---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Arsitektur Data
sesi: 3
status: siap-dikirim
tags: [klarifikasi, arsitektur-data]
---
# Daftar Klarifikasi — Arsitektur Data

> Konteks: penilaian awal hanya berbasis dokumen Pedoman Tata Kelola Data dan TI (Perdir 4/2025) dan paparan Enterprise Architecture As-Is. Platform data (ESDP) dikaji sebagai poros arsitektur. Klarifikasi berikut untuk memvalidasi kondisi aktual. Rujukan: [[13-Arsitektur-Data]].

## A. Dokumentasi & Roadmap Arsitektur
1. Apakah sudah ada dokumen arsitektur data yang memuat data building block, domain, entitas, pemetaan entitas ke proses bisnis dan aplikasi, serta diagram aliran data, sebagaimana diatur Pedoman Tata Kelola Data (Pasal 13)?
2. Apakah tersedia Enterprise Data Model lengkap? Paparan EA hanya memuat sebagian halaman.
3. Apakah sudah ada roadmap arsitektur data atau IT Master Plan terbaru pengganti ITMP 2019–2024?

## B. Sumber Data & Aplikasi
4. Untuk setiap domain data, aplikasi apa yang menjadi sumber kebenaran?
5. Apakah ada rencana modernisasi SMILE (mis. pemecahan monolitik) yang memengaruhi cara data diambil?
6. Apakah ada rencana konsolidasi jenis database (Oracle, MariaDB, Yugabyte, SQL Server, PostgreSQL)?

## C. Platform Data (Penyimpanan & Pemrosesan)
7. Edisi dan versi Cloudera apa yang digunakan, dan apakah ada rencana upgrade?
8. Apakah Data Lake dan Data Warehouse berada di satu cluster/environment yang sama, atau sudah ada pemisahan (cluster, tenant, zona akses)?
9. Apakah Data Staging (standby database berbasis Active Data Guard) juga berfungsi sebagai Operational Data Store? Apa cakupan pengadaan Oracle ODS?
10. Apakah ada kebutuhan data dengan latensi rendah (mis. hampir real-time) untuk layanan operasional atau mitra?
11. Bagaimana rencana pemanfaatan data tidak terstruktur (dokumen klaim, KYC) di platform?
12. Berapa volume data platform saat ini dan proyeksi pertumbuhannya?

## D. Analitik & Konsumsi
13. Siapa saja pengguna platform (unit, aplikasi, mitra), dan apakah ada rencana self-service analitik?
14. Apakah ada rencana konsolidasi tool BI (Tableau dan QlikSense)?

## E. Keamanan, Infrastruktur & Ketahanan
15. Bagaimana kontrol akses diterapkan per lapisan platform (staging, lake, DWH, data mart) dan per klasifikasi data?
16. Apakah platform data tercakup dalam DRP, memiliki replika di DRC, dan sudah pernah diuji? Berapa target RPO/RTO saat ini, jika ada?
17. Bagaimana kebijakan BPJS terhadap penggunaan cloud (private/public, region Indonesia) untuk data dan klasifikasi apa yang diizinkan?

## F. ESDP & Peran
18. Apa definisi dan cakupan ESDP menurut BPJS TK? Apakah ESDP dipahami sebagai platform baru atau evolusi platform eksisting?
19. Bagaimana pembagian peran antara Unit Manajemen Data dan Unit Operasional TI dalam penyusunan dan operasional arsitektur platform data?

## G. Permintaan Data / Dokumen Pendukung
- [ ] Dokumen arsitektur data / EDM lengkap
- [ ] Diagram arsitektur platform data (cluster, environment, zona) terbaru
- [ ] Spesifikasi Cloudera (edisi, versi, kapasitas, jumlah node)
- [ ] Dokumen pengadaan/arsitektur Oracle ODS, jika ada
- [ ] Data volumetri platform dan proyeksi pertumbuhan
- [ ] DRP dan hasil uji DR terakhir terkait platform data
- [ ] Kebijakan atau kajian penggunaan cloud, jika ada
- [ ] ITMP / rencana strategis TI terbaru
