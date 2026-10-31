---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Arsitektur Data
sesi: 3
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.1, 5.2, 6]
status: draft-diskusi
acuan_internal: DAMA-DMBOK v2, bab Data Architecture
sumber: [D3 paparan kapabilitas & arsitektur target, D4 kebijakan API Data Hub, R1 rekaman diskusi, D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, 00-Analisis-Dokumen-BPJS.md, 01-Inventori-Tender-BPJSTK.csv, 11-Master-Data.md, 12-Integrasi.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 77 = cetak '- 76 -')."
---

# Sesi 3 — Arsitektur Data

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI (kebijakan, kondisi yang seharusnya). **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (paparan kondisi saat ini/as-is). "hlm." merujuk pada nomor halaman PDF.

> **Kerangka sesi.** Konsisten dengan sesi 1–2: ESDP adalah platform poros, dan integrasi adalah salah satu komponennya (upstream: sumber → platform; downstream: platform → pengguna data). Blueprint menetapkan **prinsip dan parameter yang wajib didefinisikan** (mis. SLA, freshness, retensi), bukan nilainya. Pemodelan rinci (medallion, Kimball/Data Vault) dibahas di sesi 4 Pemodelan & Desain Data; di sini hanya dirujuk.

## 1. Ringkasan
- **Bentuk arsitektur as-is sudah benar, tetapi lapisannya belum dipisahkan dan belum dikelola.** D2 menggambarkan alur lengkap: Data Source → Data Integration (API, SFTP, ETL) → Data Storage (Staging → Data Lake → Data Warehouse, dengan Metadata, Data Catalog, Master Data) → Data Analytics → Data Consumption, dengan lapisan Data Security dan Data Governance (D2 hlm. 20). Komponen master data dan sebagian kapabilitas (unstructured, prescriptive) belum operasional (D2 hlm. 20, 24).
- **Data Lake dan DWH berada di satu environment Cloudera.** D2 sendiri mencatat risiko dampak serangan yang lebih besar (D2 hlm. 21). Edisi/versi Cloudera (mis. CDP) [perlu konfirmasi].
- **Komponen referensi D1 yang belum tampak di as-is:** ODS, MD&DR yang menjadi rujukan DWH, dan cubes/OLAP (D1 hlm. 77). Data Staging di D2 berupa standby database via ADG (D2 hlm. 21); apakah berfungsi sebagai ODS [perlu konfirmasi].
- **Sisi sumber kompleks dan berisiko.** SMILE adalah core yang dipakai seluruh direktorat (D2 hlm. 35), monolitik di WebLogic (D2 hlm. 37), dengan lima jenis database: Oracle (30 aplikasi), MariaDB, Yugabyte, SQL Server, PostgreSQL (D2 hlm. 37–38).
- **Kebijakan arsitektur data ada, praktik belum terbukti.** D1 Ps. 13 mewajibkan arsitektur data (DBB, domain, entitas, pemetaan ke bisnis dan aplikasi, aliran data) beserta roadmap dan analisis kesenjangan (D1 hlm. 14–15). D2 memuat DBB dan alur, tetapi dokumentasi DBB masih mengacu ITMP 2019–2024 (D2 hlm. 31) dan EDM baru sebagian (D2 hlm. 29–30).
- **Skor usulan rata-rata ±1,8** (31 Okt 2026: tetap ±1,8; AD-7 naik, AD-10 turun, lihat Pembaruan 2026-10-31).
- **Rekomendasi:** **Opsi B — ESDP sebagai platform terpadu berlapis di atas Cloudera eksisting**, dengan pemisahan zona (staging/lake, DWH, serving), ODS untuk kebutuhan latensi rendah, dan lapisan layanan ESDP untuk upstream/downstream. Opsi ini dipilih setelah dibandingkan dengan tiga opsi lain (bagian 5).

## 2. Arsitektur saat ini (as-is)

**Gambar 1.** Arsitektur data kondisi saat ini (D2 hlm. 20–26, 35–40, 50); risiko dan gap ditandai merah.

![[13-Arsitektur-Kondisi-Saat-Ini.excalidraw]]

| Lapisan | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Data source | Internal (core business, operational & supporting, corporate service) dan eksternal | D2 hlm. 20 | Baru sebagian DBB tersimpan lengkap dan terstruktur (D2 hlm. 21). Sebagian data masih diolah dengan spreadsheet (D2 hlm. 31) |
| Aplikasi & SoR | SMILE sebagai core di seluruh direktorat; aplikasi lain sebagian terhubung, sebagian tidak | D2 hlm. 35, 45 | Monolitik WebLogic, belum scalable (D2 hlm. 37) |
| Database | Oracle (30 aplikasi), MariaDB, Yugabyte, SQL Server, PostgreSQL | D2 hlm. 37–38 | Heterogenitas memengaruhi pola ingest (lihat [[12-Integrasi]]) |
| Integrasi | ADG, Talend, Qlik Replicate, API, SFTP | D2 hlm. 20–21 | Dinilai rinci di sesi 2 |
| Staging | Standby database (ADG) sebagai penyimpanan sementara sebelum Data Lake | D2 hlm. 21 | Relasi dengan ODS [perlu konfirmasi] |
| Data Lake & DWH | Cloudera; structured & unstructured; DWH, EDW, data mart; satu environment | D2 hlm. 20–21 | Unstructured belum operasional (D2 hlm. 20) |
| Master data, metadata | Master data manual tanpa tool; Dataedo (Oracle) dan Apache Atlas (Cloudera) | D2 hlm. 24, 26 | Lihat [[11-Master-Data]] |
| Analitik | Descriptive, diagnostic, predictive (berbasis use case); prescriptive belum operasional; Python, KNIME | D2 hlm. 20, 22 | – |
| Konsumsi | 17 dashboard; Tableau, QlikSense; Data Collaboration via platform eksternal beragam | D2 hlm. 22 | Analisis via request ke MDT, lambat (D2 hlm. 22) |
| Lapisan horizontal | Data Security, Data Governance | D2 hlm. 20 | Ditampilkan di gambar; penerapan per lapisan tidak dirinci |
| Infrastruktur | On-premise, virtualisasi & container; DC dan DRC colocation Tier III; backup tape tanpa restore testing berkala | D2 hlm. 50 | API Gateway muncul sebagai komponen platform (D2 hlm. 50) |

## 3. Kondisi yang seharusnya (D1)
- **Pengelolaan arsitektur data (Ps. 13):** oleh Unit Manajemen Data, sebagai pedoman integrasi, kontrol aset data, dan keselarasan dengan rencana strategis; meliputi penyusunan arsitektur dan roadmap (D1 hlm. 14).
- **Prinsip:** *quality-oriented* dan *innovation-oriented* (D1 hlm. 15).
- **Komponen wajib:** konseptual data (DBB, domain, entitas), pemetaan entitas ke arsitektur bisnis dan aplikasi, diagram aliran data (D1 hlm. 15).
- **Roadmap:** identifikasi dan evaluasi arsitektur existing, analisis kesenjangan terhadap layanan yang akan dikembangkan dan kecukupan arsitektur aplikasi (D1 hlm. 15).
- **Prinsip DWBI/big data:** fokus bisnis, desain global dengan pengembangan inkremental, transparansi, self-service, integrasi dengan metadata, kolaborasi dengan pengelolaan data (D1 hlm. 76).
- **Komponen referensi DWBI:** data integration (ETL/virtualization), staging, master data & data referensi, central warehouse, ODS, data mart, cubes/OLAP, big data platform, BI (D1 hlm. 77).
- **Kriteria pemilihan arsitektur:** kinerja & ketersediaan, keamanan & privasi, interoperabilitas software/hardware (D1 hlm. 77–78).
- **Peran:** Unit Manajemen Data menyusun dan mengimplementasikan arsitektur DWBI, berkoordinasi dengan UPTI dan Unit Operasional TI (D1 hlm. 76–77).

## 4. Tabel penilaian

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal. Setiap skor harus punya bukti.

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| AD-1. Dokumentasi arsitektur data | DBB, domain, entitas, pemetaan ke bisnis & aplikasi, aliran data; terpelihara | Komponen wajib Ps. 13(4) (D1 hlm. 15) | DBB ada (D2 hlm. 16–18); alur data as-is (D2 hlm. 20); dokumentasi DBB masih mengacu ITMP 2019–2024 (D2 hlm. 31); EDM baru 2 dari 11 halaman tersedia (D2 hlm. 29–30) | Pemetaan entitas ke aplikasi dan aliran data per domain belum lengkap; pemeliharaan tidak terbukti | 2 |
| AD-2. Roadmap & analisis kesenjangan | Roadmap arsitektur data selaras renstra, ditinjau berkala | Ps. 13(5) (D1 hlm. 15) | D2 adalah identifikasi as-is; roadmap arsitektur data tidak ada di dokumen | Roadmap belum ada (studi ini mengisinya) | 2 |
| AD-3. Lapisan sumber & SoR | SoR ditetapkan per domain; sumber dapat diakses tanpa membebani produksi | Repository master tunggal (D1 hlm. 88) | SMILE core (D2 hlm. 35), monolitik (D2 hlm. 37), 5 jenis DB (D2 hlm. 37–38), aplikasi tidak terhubung ke SMILE (D2 hlm. 45) | SoR per domain belum ditetapkan; heterogenitas tinggi | 2 |
| AD-4. Lapisan integrasi | Pola standar upstream/downstream | Ps. 66 (D1 hlm. 50–51); komponen data integration (D1 hlm. 77) | Lihat [[12-Integrasi]]: rata-rata upstream ±1,6, downstream ±1,6 | Lihat sesi 2 | 2 |
| AD-5. Lapisan penyimpanan (staging, lake, DWH) | Zona terpisah dengan kontrol akses dan isolasi berbeda | Staging, central warehouse, big data platform (D1 hlm. 77) | Staging (ADG) → Data Lake → DWH di Cloudera, satu environment, risiko dampak serangan lebih besar (D2 hlm. 21); unstructured belum operasional (D2 hlm. 20) | Zona belum terisolasi | 2 (31 Okt 2026, tetap): raw, model, dan data bersih hanya dibedakan skema (R1 [S 00:44:54–00:45:06]) |
| AD-6. ODS / operational serving | Penyimpanan latensi rendah, non-historis, untuk kebutuhan operasional | ODS (D1 hlm. 77) | Tidak disebut; staging standby ADG (D2 hlm. 21). Riset publik: tender "Oracle ODS" | Komponen ODS tidak tampak | 1 [perlu konfirmasi] |
| AD-7. Master data & metadata dalam arsitektur | MD&DR menjadi rujukan DWH; metadata terintegrasi | D1 hlm. 76–77 | Master data manual, big data tidak mengacu master data (D2 hlm. 24); dua katalog terpisah (D2 hlm. 26) | Lihat [[11-Master-Data]] | 1 → **2** (31 Okt 2026): POC MDM (D3-02) dan Dataedo aktif (R1 [P 00:21:08–00:21:14]); lineage dan katalog merah (D3-01) |
| AD-8. Lapisan analitik & konsumsi | Self-service terkontrol, produk data, BI terpadu | Self-service, transparansi (D1 hlm. 76); jenis analitik (D1 hlm. 78) | 17 dashboard, Tableau & QlikSense, Python & KNIME (D2 hlm. 22); prescriptive belum operasional (D2 hlm. 20); request ke MDT lambat (D2 hlm. 22) | Self-service dan katalog produk data belum ada | 2 |
| AD-9. Keamanan & tata kelola sebagai lapisan horizontal | Kontrol per lapisan dan per klasifikasi data | Klasifikasi Biasa/Terbatas/Rahasia (D1 hlm. 93–94); kriteria keamanan & privasi (D1 hlm. 78) | Lapisan digambarkan (D2 hlm. 20); risiko satu environment dan pengiriman eksternal (D2 hlm. 21–22) | Penerapan kontrol per lapisan tidak terbukti | 2 |
| AD-10. Infrastruktur & ketahanan | Kapasitas terencana, DR teruji, pemisahan environment | Kriteria kinerja & ketersediaan (D1 hlm. 78); DRP diuji (D1 Ps. 42, lihat 00-Analisis) | On-prem, virtualisasi/container, DC & DRC Tier III; tape tanpa restore testing berkala; monitoring belum dipetakan ke aplikasi (D2 hlm. 50) | DR untuk platform data dan restore test belum terbukti | 2 → **1** (31 Okt 2026): belum ada komponen Cloudera di DRC (R1 [S 00:35:03–00:37:39]), bertentangan dengan D3-01 [perlu konfirmasi]; Retention Policy merah, Restore Testing tanpa status (D3-01) |
| AD-11. Prinsip arsitektur | Prinsip tertulis, dipakai dalam keputusan desain | Quality- & innovation-oriented (D1 hlm. 15); prinsip DWBI (D1 hlm. 76) | Tidak ada bukti prinsip dipakai sebagai kriteria keputusan | Prinsip belum dioperasionalkan | 2 |

**Rata-rata: ±1,8.**

## 5. Prinsip arsitektur (usulan Blueprint)
Prinsip ini menurunkan D1 hlm. 15 dan 76 menjadi kriteria keputusan, dan dapat menjadi masukan revisi Perdir.
1. **Platform tunggal, zona terpisah.** Satu ESDP dengan zona yang terisolasi (staging/lake, DWH, serving), bukan satu environment datar (menjawab D2 hlm. 21).
2. **SoR tetap di aplikasi sumber.** Platform tidak menjadi tempat transaksi; setiap domain punya SoR yang ditetapkan.
3. **Master data sebagai rujukan.** Seluruh dimensi kunci di DWH mengacu ke MDM (D1 hlm. 77; D2 hlm. 24).
4. **Metadata-first.** Setiap dataset dan interface terdaftar di katalog, dengan lineage (D1 hlm. 76).
5. **Keamanan berbasis klasifikasi** di setiap zona (D1 hlm. 93–94).
6. **Integrasi melalui komponen ESDP** dengan prinsip upstream/downstream sesi 2; tanpa Direct-to-DB.
7. **Desain global, implementasi inkremental** sesuai prioritas bisnis (D1 hlm. 76).
8. **Parameter wajib didefinisikan per komponen**, bukan nilai tetap di Blueprint: SLA ketersediaan, freshness, latensi, RPO/RTO, retensi, kapasitas, klasifikasi.
9. **Reuse investasi eksisting sebelum menambah platform**, kecuali ada gap yang tidak bisa ditutup.

## 6. Opsi arsitektur target

| Opsi | Deskripsi |
|---|---|
| A. Optimasi as-is | Pertahankan Cloudera satu environment; tambah tata kelola, katalog, dan master data |
| B. ESDP terpadu berlapis di atas Cloudera | Cloudera sebagai inti; zona terpisah (staging/lake, DWH, serving); ODS untuk latensi rendah; lapisan layanan ESDP (integrasi upstream/downstream, API, katalog); MDM sebagai komponen rujukan |
| C. Dua platform terpisah | DWH relasional (mis. Oracle) untuk pelaporan terstruktur + Cloudera untuk big data/lake |
| D. Hybrid/cloud | Sebagian beban (analitik, DR, sandbox) ke private/public cloud region Indonesia; inti tetap on-prem |

**Perbandingan (skala: ++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria | A | B | C | D |
|---|---|---|---|---|
| Kesesuaian dengan komponen referensi D1 (hlm. 77) | − (ODS, MD&DR tidak ada) | ++ | + | + |
| Menutup risiko satu environment (D2 hlm. 21) | −− | + | ++ | + |
| Reuse investasi (Cloudera, Talend, Qlik, Oracle) | ++ | ++ | + | 0 |
| Kompleksitas operasi & kebutuhan SDM | + | 0 | −− (dua stack, duplikasi data) | − |
| Dukungan downstream (API, DaaS, freshness rendah) | − | ++ | 0 | + |
| Kepatuhan lokasi data & regulasi (PP 71/2019, UU PDP) [perlu konfirmasi kebijakan cloud] | ++ | ++ | ++ | − |
| Skalabilitas | 0 | + | 0 | ++ |
| Waktu ke manfaat | ++ | + | − | − |
| Risiko implementasi | + | 0 | − | − |

**Rekomendasi: Opsi B.** Paling sesuai D1, menutup risiko utama D2 hlm. 21 melalui pemisahan zona, dan memakai ulang investasi eksisting. Opsi A tidak menutup gap ODS dan isolasi. Opsi C menyelesaikan isolasi dengan biaya dua stack dan duplikasi data. Opsi D dicatat sebagai **opsi pelengkap** (mis. DR, sandbox analitik) yang dinilai pada panduan pemilihan teknologi & deployment (Blueprint 5.7, parking lot sesi Lensa), menunggu kebijakan cloud BPJS [perlu konfirmasi].

**Gambar 2.** Arsitektur target Opsi B — ESDP berzona di atas Cloudera, dengan MDM sebagai rujukan dan Opsi D sebagai pelengkap opsional.

![[13-Arsitektur-Target-Opsi-B.excalidraw]]

## 7. Batas peran komponen

| Komponen | Peran | Bukan peran |
|---|---|---|
| SoR (SMILE dan aplikasi operasional) | Transaksi dan entri data; pemilik skema; sumber upstream | Bukan hub integrasi untuk analitik; bukan tempat analitik |
| MDM | Identitas, golden record, kode referensi; rujukan dimensi DWH | Bukan tempat transaksi atau gudang analitik |
| ESDP (platform poros) | Wadah arsitektur: komponen integrasi (upstream/downstream), zona penyimpanan, ODS/serving, katalog & metadata, keamanan, layanan data | Bukan pengganti SoR. Definisi resmi [perlu konfirmasi] |
| Cloudera | Mesin penyimpanan & pemrosesan di dalam ESDP (lake, DWH, pemrosesan) | Bukan sumber operasional real-time; tidak diakses langsung oleh mitra; bukan pembentuk golden record |
| ODS | Data terkini latensi rendah untuk kebutuhan operasional dan layanan downstream | Bukan penyimpanan historis |
| Metadata (Dataedo/Atlas) | Katalog, lineage, definisi data contract | – |

## 8. Keterkaitan dengan Blueprint
- **3.1–3.3 (as-is & gap):** bagian 2–4 dokumen ini.
- **3.4 (prinsip arsitektur):** bagian 5.
- **4 (arsitektur target):** bagian 6–7; pemodelan lapisan (medallion) dan pola DWH dirinci di sesi 4.
- **5.1–5.2 (komponen & integrasi):** batas peran (bagian 7) dan [[12-Integrasi]].
- **6 (roadmap):** tema besar per tahun (usulan, untuk disepakati):
  - **Tahun 1 — Fondasi:** prinsip & parameter, inventaris sumber/interface, pemisahan zona, katalog terpadu.
  - **Tahun 2 — Layanan:** ODS/serving, lapisan layanan downstream, MDM consolidation.
  - **Tahun 3 — Optimasi:** self-service, data sebagai layanan, event/real-time, evaluasi hybrid.

## 9. Pertanyaan terbuka untuk BPJS
1. Edisi dan versi Cloudera apa yang dipakai (mis. CDP Private Cloud Base), dan apakah ada rencana upgrade?
2. Apakah Data Lake dan DWH benar-benar di satu cluster/environment, atau ada pemisahan logis (mis. tenant, zona)?
3. Apakah Data Staging (standby ADG) berfungsi sebagai ODS, dan apa cakupan tender Oracle ODS?
4. Apakah ada dokumen EDM lengkap (11 halaman) dan pemetaan entitas ke aplikasi?
5. Apakah sudah ada roadmap arsitektur data atau ITMP terbaru pengganti ITMP 2019–2024?
6. Apakah platform data tercakup dalam DRP dan sudah pernah diuji?
7. Bagaimana kebijakan BPJS terhadap penggunaan cloud untuk data (lokasi, klasifikasi yang diizinkan)?
8. Berapa volume data platform saat ini dan proyeksi pertumbuhan (terkait target 70 juta peserta, D2 hlm. 37)?
9. Apa definisi dan cakupan ESDP menurut BPJS TK, dan apakah ESDP dipahami sebagai platform baru atau evolusi platform eksisting?

## 10. Poin diskusi (ikpu)
- [ ] Sepakati Opsi B sebagai rekomendasi, dengan Opsi D sebagai pelengkap yang dinilai di Blueprint 5.7.
- [ ] Putuskan apakah ODS menjadi komponen wajib target (menjawab D1 hlm. 77) atau cukup formalisasi standby ADG.
- [ ] Sepakati daftar parameter wajib per komponen (SLA, freshness, latensi, RPO/RTO, retensi, kapasitas) yang ditetapkan Blueprint tanpa nilai.
- [ ] Validasi tema roadmap tahunan (Fondasi → Layanan → Optimasi) dan apakah perlu dipecah per semester.
- [ ] Validasi skor, terutama AD-6 (ODS) dan AD-7 (master data & metadata).
- [ ] Prioritaskan pertanyaan 2, 3, 7, dan 9 untuk sesi dengan Unit Manajemen Data dan Unit Operasional TI.


## Pembaruan 2026-10-31

**Bukti baru (kondisi saat ini):**
- **Arsitektur target BPJS (D3-07)** memuat lapisan Data Source → Data Integration (API, SFTP, ETL Tools) → Data Storage (Data Staging → Data Lake dengan Data Classification → Data Warehouse/EDW dengan Metadata, Data Catalog, Data Reference, Data Mart) → Data Analytics → Data Insight, dengan Data Security dan Data Governance sebagai lapisan horizontal. Lapisan ini **sama dengan D2 hlm. 20**; D3-07 tidak menampilkan ODS, MDM, maupun lapisan serving/API terpisah. Opsi B (§6) tetap menambah ketiganya.
- **Penyimpanan:** CDC masuk Kudu (update per jam), lalu dibentuk Parquet (≥1–2 jam) untuk analitik; HDFS; data mart sebagian besar Kudu/Parquet hasil Talend (R1 [S 00:17:55–00:19:21; S 00:29:19–00:31:48]). Oracle tetap dipakai untuk kebutuhan tertentu (finance) (R1 [S 00:45:44–00:47:04]).
- **Pemisahan lapis hanya berbasis skema** (R1 [S 00:44:54–00:45:06]); BPJS menerima pendekatan medallion (R1 [S 00:44:12–00:44:48]).
- **Konsumsi operasional dari big data** ada tetapi sedikit (mis. report untuk core system) (R1 [S 00:21:48–00:22:42]); relevan untuk kebutuhan ODS (AD-6).
- **Status kapabilitas (D3-01):** kuning untuk CDC, batch, ETL, data mart, BI, enkripsi, masking, archive; merah untuk data catalog, lineage, audit trail, retention policy, API Management, message broker, workflow orchestration, event streaming; tanpa status untuk Metadata Management, Data Profiling, Restore Testing. Storage/warehouse dan API Gateway dalam kotak "Belum tersedia".
- **Konflik DRC:** D3-01 menggambarkan "DRC (Active)" dengan infrastruktur 20 node (Coordinator, Worker, MDM Engine); R1 [S 00:35:03–00:37:39] menyatakan Cloudera di DRC belum ada komponennya (hanya rack dan uji coba 1 mesin), dan hubungan DC–DRC belum didefinisikan [perlu konfirmasi]. Penilaian memakai pernyataan yang lebih konservatif sampai dikonfirmasi.
- **DMZ belum terdefinisi** (R1 [S 00:38:53–00:40:30]); posisi Data Hub (D4: DMZ, B2B, REST/JSON, 3scale) dibahas di [[12-Integrasi]] §5.6.
- Ekspektasi kajian: horizon 3 tahun, memaksimalkan teknologi yang ada (R1 [S 00:32:26–00:32:36; S 00:47:28–00:47:46]); sejalan dengan Opsi B.

| Aspek | Skor lama → baru | Alasan / sumber |
|---|---|---|
| AD-1. Dokumentasi arsitektur | 2 → 2 | D3-07 dan model konseptual (D3-04, D3-06) ada; tidak memuat ODS/MDM/serving dan model belum mencakup JKP |
| AD-5. Lapisan penyimpanan | 2 → 2 | Pemisahan hanya skema (R1) |
| AD-6. ODS | 1 [pk] → 1 [pk] | Konsumsi operasional kecil dari big data (R1); komponen ODS tetap tidak tampak |
| AD-7. Master data & metadata | 1 → 2 | POC MDM (D3-02); Dataedo aktif; lineage/katalog merah (D3-01) |
| AD-9. Keamanan horizontal | 2 → 2 | Enkripsi/masking kuning, audit trail merah (D3-01) |
| AD-10. Infrastruktur & ketahanan | 2 → 1 | DRC platform belum ada menurut R1; retensi merah; restore test tanpa status (D3-01) [perlu konfirmasi] |

**Rata-rata baru: ±1,8** (tetap).
