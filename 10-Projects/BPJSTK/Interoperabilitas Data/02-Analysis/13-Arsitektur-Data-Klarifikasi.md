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
9. Apakah Data Staging (standby database berbasis Active Data Guard) juga berfungsi sebagai Operational Data Store? Apa cakupan pengadaan Oracle ODS?
10. Apakah ada kebutuhan data dengan latensi rendah (mis. hampir real-time) untuk layanan operasional atau mitra? *(Sebagian terjawab, 31 Okt 2026: streaming belum ada dan baru diadopsi bila ada kebutuhan bisnis (R1 [S 00:32:22–00:33:31]); kebutuhan per layanan belum dipetakan)*
11. Bagaimana rencana pemanfaatan data tidak terstruktur (dokumen klaim, KYC) di platform?
12. Berapa volume data platform saat ini dan proyeksi pertumbuhannya? *(Sebagian terjawab, 31 Okt 2026: volume belum dijawab; BPJS meminta fokus pada data dulu (R1 [P 01:30:27–01:30:45]))*

## D. Analitik & Konsumsi
14. Apakah ada rencana konsolidasi tool BI (Tableau dan QlikSense)?

## E. Keamanan, Infrastruktur & Ketahanan
15. Bagaimana kontrol akses diterapkan per lapisan platform (staging, lake, DWH, data mart) dan per klasifikasi data?
16. Apakah platform data tercakup dalam DRP, memiliki replika di DRC, dan sudah pernah diuji? Berapa target RPO/RTO saat ini, jika ada? *(Sebagian terjawab, 31 Okt 2026: R1 [S 00:35:03–00:37:39] menyatakan belum ada komponen Cloudera di DRC, bertentangan dengan D3-01; lihat pertanyaan baru)*
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

## Terjawab (pembaruan 31 Okt 2026)
> Pertanyaan berikut dipindahkan dari daftar di atas karena sudah dijawab. Nomor asli dipertahankan sebagai rujukan.

| No. asli | Pertanyaan | Status | Jawaban & sumber |
|---|---|---|---|
| 8 | Apakah Data Lake dan Data Warehouse berada di satu cluster/environment yang sama, atau sudah ada pemisahan (cluster, tenant, zona akses)? | **Terjawab** | Satu environment; raw/staging, model, dan data bersih hanya dibedakan skema (R1 [S 00:44:54–00:45:06]). |
| 13 | Siapa saja pengguna platform (unit, aplikasi, mitra), dan apakah ada rencana self-service analitik? | **Terjawab** | Pengguna analitik kompleks diarahkan ke tabel Parquet; core system mengonsumsi sedikit data (mis. report); banyak cube untuk finance (R1 [S 00:21:36–00:22:42; S 00:30:25–00:31:00]). Rencana self-service belum disebut. |

## Pertanyaan baru (31 Okt 2026)
20. **Konflik status DRC:** D3-01 menggambarkan DRC aktif dengan 20 node (Coordinator, Worker, MDM Engine), sedangkan R1 menyatakan belum ada komponen Cloudera di DRC. Mana yang benar? Apakah 20 node itu cluster produksi di DC, cluster DRC, atau rencana?
21. **Data Hub:** apakah Data Hub akan dibangun, di dalam environment big data atau di zona serving terpisah (DMZ)? Siapa pemilik dan kapan jadwalnya?
22. **DMZ:** kapan zona jaringan (DMZ/green zone) dan letak API Gateway akan didefinisikan, dan oleh unit mana?
23. **Volume trafik & kapasitas:** volume data per lapis (Kudu, Parquet, HDFS) dan volume permintaan API/SFTP saat ini serta proyeksinya.
24. **Status POC MDM terhadap target Okt 2026:** di mana mesin MDM dijalankan, dan apakah target Okt 2026 tercapai?
25. **Arti status "Belum tersedia" di D3-01:** apakah storage/warehouse dan API Gateway pada kotak itu merujuk ke komponen baru untuk golden record (SSOT) atau ke platform eksisting?
