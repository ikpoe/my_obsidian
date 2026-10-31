# Analisis Dua Dokumen BPJS Ketenagakerjaan untuk Studi "Reviu dan Perancangan Arsitektur Interoperabilitas Data Ketenagakerjaan" / Blueprint ESDP

Disusun: 14 Okt 2026 (WIB). Kerangka acuan: DAMA-DMBOK v2.
Konvensi sitasi: **D1 hlm. X** = halaman PDF dokumen 1 (110 hlm); **D2 hlm. X (slide Y)** = halaman PDF dokumen 2 (51 hlm), Y = nomor slide asli yang tercetak di pojok kanan bawah.
Catatan: kedua PDF memuat watermark diagonal penerima/tanggal ("PIRANTI SEJAHTERA INTI OKT 2026" terbaca jelas di D2; D1 memuat pola watermark serupa). Perlakukan sebagai dokumen terbatas. D1 adalah hasil pindai (scanner KM_751i) dengan OCR; beberapa kata ter-OCR kurang sempurna.

---

## 1. Ringkasan identitas dokumen

| | D1 | D2 |
|---|---|---|
| Judul | Salinan **Peraturan Direksi BPJS Ketenagakerjaan No. 4/PERDIR.06/042025 tentang Pedoman Tata Kelola Data dan Teknologi Informasi** (D1 hlm. 1) | **"Enterprise Architecture As-Is"**: assessment arsitektur bisnis, data, aplikasi, infrastruktur, plus pemetaan COBIT 2019 (D2 hlm. 1–51) |
| Penulis/penetap | Ditetapkan Direktur Utama Anggoro Eko Cahyo, Jakarta, **23 April 2025**; salinan disahkan Asdep Advokasi Hukum (D1 hlm. 63, 86, 92, 110) | Metadata PDF: author "Donny Satrio", dibuat 20 Okt 2026. Penyusun konsultan/unit tidak disebut eksplisit. Slide memakai frasa "kami menyusun… berdasarkan pemahaman kami" (D2 hlm. 50, slide 452), jadi kemungkinan besar hasil kerja konsultan (inferensi) |
| Sifat | Regulasi internal yang mengikat (kebijakan) | Laporan asesmen kondisi kini (deskriptif). Berupa **potongan** dari dokumen besar: nomor slide melompat 353→555, sehingga banyak slide (mis. EDM 4/11 s.d. 11/11, Application Portfolio Assessment, detail infrastruktur) **tidak disertakan** |
| Konteks | Mencabut 4 Perdir lama: Data Governance Kepesertaan 2015, Tata Kelola TI 2021, Tata Kelola Data 2022 (dan 2 perubahannya, terakhir 07/2024), Komite Pengarah TI 2022 (D1 hlm. 1, 62) | Menyebut "IT Master Plan Tahun 2019-2024 (update)" sebagai acuan DBB saat ini (D2 hlm. 31, slide 406), sehingga kemungkinan bagian dari penyusunan ITMP 2025–2027 (inferensi; tender ITMP 2025–2027 tercatat publik Mei 2024) |

---

## 2. D1: Perdir 4/PERDIR.06/042025 (Pedoman Tata Kelola Data dan TI)

### 2.1 Struktur
- **Batang tubuh (D1 hlm. 1–63), 80 pasal:**
  - Bab I Ketentuan Umum
  - Bab II Struktur Tata Kelola (KPTI, UPTI)
  - Bab III Perencanaan & Implementasi (ITMP, arsitektur data, pemodelan, metadata, DWBI/big data, kualitas, change, release)
  - Bab IV Operasional (layanan TI, katalog data, master data & data referensi, insiden, CMDB, kapasitas, DRP, pihak ketiga, jaringan, database)
  - Bab V SMKI (keamanan informasi, termasuk keamanan interoperabilitas)
  - Bab VI Kolaborasi & Pemanfaatan Data
  - Bab VII Monev
  - Bab VIII–IX Peralihan & Penutup
- **Lampiran I (D1 hlm. 64–86):** SDLC (COTS/tailor-made, waterfall/agile, DevSecOps), metadata, DWBI/big data, manajemen kualitas data.
  - Catatan: hlm. 64 dan 65 merupakan pindaian ganda.
- **Lampiran II (D1 hlm. 87–92):** master data & data referensi; penyimpanan & operasional database (termasuk archiving).
- **Lampiran III (D1 hlm. 93–110):** keamanan aset, klasifikasi data, pengendalian akses/PAM, fisik, operasional TI, komunikasi/email.

### 2.2 Isi kunci (dengan halaman)
- **Dasar hukum** mencakup UU ITE, UU 24/2011 BPJS, **UU 27/2022 PDP**, PP 85/2013 hubungan antar lembaga BPJS, **PP 71/2019**, **Perpres 39/2019 Satu Data**, **Inpres 4/2025 Data Tunggal Sosial dan Ekonomi Nasional**, POJK 13/2018, **POJK 4/POJK.05/2021 manajemen risiko TI LJK Nonbank**, POJK 6/2022 (D1 hlm. 1–3).
- **Rujukan standar:** teknis tata kelola mengacu ke **COBIT dan DMBOK** (Pasal 3(3), D1 hlm. 6).
- **Struktur tata kelola:**
  - Direktur fungsi data & TI, KPTI, UPTI, Unit Bisnis, Kanwil, Kancab (Pasal 4, D1 hlm. 7).
  - KPTI diketuai Direktur data & TI dan rapat tiap semester (Pasal 6–7, D1 hlm. 7–9).
  - UPTI terdiri dari Unit Pengembangan TI, Unit Operasional TI, dan **Unit Manajemen Data** (Pasal 1, D1 hlm. 4).
- **Mandat Unit Manajemen Data** (Pasal 8(3), D1 hlm. 9): kualitas data, pemutakhiran/koreksi, **master data & data referensi**, **DWBI/big data**. Juga pemilik:
  - arsitektur data (Pasal 13)
  - EDM (Pasal 14)
  - metadata (Pasal 15)
  - katalog data (Pasal 31)
  - MD&DR (Pasal 32)
  - klasifikasi data dan matriks kontrol akses data (Lamp. III, D1 hlm. 93, 100)
- **Kantor cabang = "data producer"** yang wajib input data akurat dan memperbaiki kualitas (Pasal 8(6), D1 hlm. 10).
- **ITMP minimal 3 tahun** dengan muatan wajib, termasuk arsitektur data dan tata kelola data; direviu tahunan (Pasal 10, D1 hlm. 11–13).
- **Arsitektur data** (Pasal 13, D1 hlm. 14–15):
  - komponen: data building block → domain → entitas, pemetaan entitas ke arsitektur bisnis dan aplikasi, diagram aliran data
  - roadmap arsitektur data dengan gap analysis
- **Pemodelan:** EDM dikelola Unit Manajemen Data. Unit Pengembangan TI membuat Physical Data Model yang wajib sesuai Kamus Data dan EDM, lalu direviu Unit Manajemen Data (Pasal 14, D1 hlm. 16–17).
- **Metadata:** business glossary dan kamus data (11 atribut standar), dievaluasi minimal setahun sekali (Pasal 15; Lamp. I.B, D1 hlm. 17, 74–75).
- **DWBI/Big Data** (Lamp. I.C, D1 hlm. 75–80):
  - prinsip: fokus bisnis, desain global, pengembangan inkremental, self-service, integrasi metadata
  - **komponen referensi**: data integration (ETL/virtualization), staging, **MD&DR**, central warehouse, **operational data store (ODS)**, data mart, cubes/OLAP, **big data platform/data lake**, BI (D1 hlm. 77)
  - Unit Manajemen Data menyusun dan mengimplementasikan arsitektur; Unit Operasional TI mengoperasikan dan mengamankan (D1 hlm. 76–77)
- **Kualitas data** (Lamp. I.D, D1 hlm. 80–86):
  - 6 dimensi: completeness, accuracy, uniqueness, validity, timeliness, consistency
  - tahapan: CDE, formula, skala; **contoh formula** termasuk NIK 16 digit dan konsistensi data kanal X vs **SMILE** (D1 hlm. 84)
  - skala Low 0–60, Medium 61–90, High 91–100 (D1 hlm. 85)
  - laporan bulanan ke Kanwil dan evaluasi triwulanan (D1 hlm. 83)
- **Katalog data:** berbasis metadata, self-service, aman sesuai klasifikasi (Pasal 31, D1 hlm. 29–30).
- **Master data & data referensi** (Pasal 32, D1 hlm. 30; Lamp. II.A, D1 hlm. 87–89):
  - prinsip: shared, ownership, kualitas, perubahan terkendali
  - arsitektur MD&DR memuat fungsi identitas/konsolidasi, create/update, sharing
  - **penambahan/pembaruan master data dilakukan terpusat di satu master data repository**
  - Unit Pengembangan TI **wajib** memakai master data
  - ada peran "data liaison"
- **Database:** evaluasi teknologi DB dengan 10 kriteria; backup/recovery oleh Unit Operasional TI; **database archiving** berbasis profiling (Lamp. II.B, D1 hlm. 89–92).
- **SMKI:** CISO dan ISM (Pasal 50, D1 hlm. 40–42); kriptografi AES/SHA minimal 256 bit, HTTPS, key management terpusat (Pasal 57, D1 hlm. 44–45).
- **Klasifikasi data 3 tingkat:** Biasa, Terbatas, Rahasia, beserta kontrolnya (enkripsi dan pertukaran terenkripsi untuk Rahasia) (Lamp. III.A.2, D1 hlm. 93–94).
- **Matriks kontrol akses data CRUD**; delete hanya berupa soft-delete (D1 hlm. 100–101).
- **PAM** (D1 hlm. 98–99).
- **Log:** minimal user id, IP, aktivitas, waktu; admin tidak boleh menghapus log (D1 hlm. 106–107).
- **Pengujian keamanan:** VA bulanan, pentest tahunan (D1 hlm. 107).
- **Keamanan interoperabilitas (Pasal 66, D1 hlm. 50–51), sangat relevan:**
  - akses interoperabilitas eksternal (prod/dev/test) wajib disetujui organisasi keamanan informasi dengan lampiran **PKS/MoU, IP address, kebutuhan operasional, NDA**
  - teknologi pertukaran data dilakukan **melalui jaringan host-to-host** berupa **(a) "secure reusable service and shared processes berbasis web service"** dan **(b) SFTP**
  - teknologi lain wajib direviu dan dikaji keamanannya
  - kontrol web service: authN/authZ, perimeter, **whitelist IP di firewall**
- **Kolaborasi & Pemanfaatan Data (Bab VI, D1 hlm. 53–58):**
  - tujuan termasuk Satu Data Indonesia
  - skema: verifikasi data, data sebagai layanan (data-as-a-service), pertukaran data, skema lain
  - mitra KL dan Non-KL; kriteria Non-KL: SMKI, teknologi integrasi, assessment oleh Unit Operasional TI dan Unit Manajemen Data, PSE/OJK untuk sektor keuangan
  - **PKS wajib memuat tujuan, metode integrasi, elemen data, keamanan, narahubung** (Pasal 71)
  - **penentuan "value data"** (cost/market/value-based/historical approach) dievaluasi tiap 2 tahun (Pasal 72)
  - sampel data untuk piloting wajib didahului NDA (Pasal 73)
- **Peralihan:** PKS pemanfaatan data wajib diadendum paling lambat 6 bulan setelah penetapan, yaitu sekitar Okt 2025 (Pasal 78, D1 hlm. 61–62).
- **Manajemen layanan TI:**
  - prioritas insiden/permintaan dengan SLA penyelesaian: urgent 8 jam kerja; immediate 1 hari; high 3 hari; medium 5 hari; low 7 hari (Pasal 37, D1 hlm. 32–33)
  - CMDB (Pasal 38)
  - IT DRP diuji berkala (Pasal 42)
  - pemisahan environment dev/test/staging/prod (D1 hlm. 104–105)

---

## 3. D2: Enterprise Architecture As-Is

### 3.1 Struktur
- **E1 Arsitektur Bisnis** (D2 hlm. 2–12; slide 354–374): business building block 3 level (hlm. 4–6) dan value chain utama plus breakdown untuk kepesertaan, investasi, klaim/manfaat, pelayanan, dengan penanda pain point (A.1–A.12, B.1–B.10, C.1–C.8, D.1–D.3). Detail pain point tidak terbaca di teks dan sebagian slidenya tidak disertakan.
- **E2 Arsitektur Data** (D2 hlm. 13–31; slide 379–406):
  - Data Building Block L0–L3 (hlm. 15–18)
  - kapabilitas data as-is (hlm. 20–22)
  - MDM (hlm. 23–24)
  - metadata (hlm. 25–26)
  - DQM (hlm. 27–28)
  - EDM (hanya 2/11 dan 3/11 di hlm. 29–30; ringkasan di hlm. 31)
- **E3 Arsitektur Aplikasi** (D2 hlm. 32–45; slide 407–423):
  - metodologi (hlm. 34)
  - pemetaan aplikasi per direktorat (hlm. 35)
  - proses tanpa aplikasi (hlm. 36)
  - teknologi (hlm. 37)
  - framework (hlm. 38)
  - integrasi internal (hlm. 39) dan eksternal (hlm. 40)
  - ABB (hlm. 42–43)
  - landscape (hlm. 45)
- **E4 Arsitektur Infrastruktur** (D2 hlm. 46–50; slide 446–452): IBB 5 layer.
- **Pemetaan COBIT 2019** per jabatan (D2 hlm. 51; slide 555), bersumber dari **Perdir No. 2/Perdir.04/012024** (struktur organisasi).

### 3.2 Isi kunci
- **Alur data as-is** (D2 hlm. 20, slide 386; diagram dicek visual):
  - Data Source (internal: core business, operational/supporting, corporate service; external) → Data Integration (**API, SFTP, ETL Tools**) → Data Storage (**Data Staging → Data Lake (structured; unstructured\*) → Data Warehouse** dengan **Metadata, Data Catalog, Master Data**, EDW, data mart) → Data Analytics (descriptive, diagnostic, predictive, prescriptive\*) → Data Consumption (executive dashboard, management/operational report, interactive dashboard, ad-hoc, **Data Collaboration (External)**)
  - lapisan horizontal Data Security dan Data Governance
  - \* = "belum digunakan secara operasional": unstructured data dan prescriptive analytics
- **Integrasi data** (D2 hlm. 21, slide 387):
  - replikasi memakai **Oracle Active Data Guard (ADG)**
  - **Talend** sebagai ETL dari **MariaDB, Oracle, HDFS**
  - **Qlik Replicate** untuk replikasi ke Data Lake
  - pertukaran eksternal via **API**; **bulk via SFTP**, khususnya untuk PKS dengan K/L
  - risiko: kebocoran, keterlambatan, gagal transfer
- **Penyimpanan** (D2 hlm. 21):
  - data lewat ETL disimpan dulu di **Data Staging (standby database)**, lalu ke Data Lake
  - **Data Lake dan DWH sama-sama di Cloudera, dalam satu environment**; risiko dampak keamanan lebih besar
  - DWH dipecah menjadi data mart per unit bisnis
- **Analitik** (D2 hlm. 22, slide 388):
  - Python dan **KNIME**
  - predictive per use case; prescriptive belum
  - permintaan analisis harus lewat **MDT** (Manajemen Data/Data Team) sehingga lama
  - **17 dashboard** (Pengadaan, Kualitas Data, Keagenan, Command Center, Pelayanan, Anggaran, Pembayaran, …)
  - BI: **Tableau dan QlikSense**
  - **Data Collaboration memakai "platform eksternal yang beragam sesuai mitra"**
- **MDM** (D2 hlm. 23–24, slide 389–390):
  - **pengelolaan master data masih manual, tanpa MDM tool**
  - big data tidak mengacu ke master data, sehingga belum tentu single source of truth
  - **belum ada SOP**
  - **fungsi pengelola master data belum didefinisikan**
  - hlm. 23 berupa ilustrasi target: MDM tool dikelola Data Steward, dengan Data Architect (MDT)
- **Metadata** (D2 hlm. 25–26):
  - glossary dan kamus data sesuai Perdir
  - **Dataedo untuk data di Oracle; Apache Atlas untuk metadata dan lineage di Cloudera**
  - tool belum bisa memberi insight saat ada struktur data baru
  - definisi istilah dan elemen data berbeda antar bidang/skema
  - **kewenangan perubahan elemen data tidak jelas**
- **Kualitas data** (D2 hlm. 27–28):
  - prosedur: metrik → profiling → laporan → penilaian → cleansing → tindak lanjut → monev
  - masih ada duplikasi, inkonsistensi, format invalid
  - **automated DQ tool belum punya fitur rekomendasi perbaikan** (tool tidak dinamai; inferensi: Talend DQ)
  - perbaikan bad data di cabang belum optimal; awareness rendah
- **EDM** (D2 hlm. 29–31):
  - relasi domain Kepesertaan dan Pelayanan; ada legenda "entitas yang belum diolah tersistem"
  - DBB masih mengacu ke **ITMP 2019–2024 (update)**
  - sebagian data masih di spreadsheet
  - banyak alur antar proses belum terhubung
- **Data Building Block** (D2 hlm. 16–18): blok **Corporate Planning, Business Services, Enterprise Support**, dengan ratusan entitas, mis.:
  - Data Primer/Sekunder Peserta
  - TK PU/BPU/Jakon/PMI
  - Pemberi Kerja/BU
  - Klaim JHT/JKK/JKM/JP/JKP
  - Saldo JHT
  - Iuran, Jurnal Transaksi
  - Investasi (saham, sukuk, reksadana)
  - Aktuaria
  - Data Audit Trail, Data System Log, Data Knowledge Business Glossary
  
  Ini bahan langsung untuk domain ESDP.
- **Aplikasi** (D2 hlm. 35, 39, 45):
  - **SMILE = core system**, dipakai di semua direktorat
  - aplikasi lain: JMO, SIPP Online/Mitra, Lapak Asik, E-PLKK, E-Klaim PMI, POM/POM PMI, PERISAI, E-Jakon, Kiosk, Contact Center, SIDIA, OSS, OTISTA, DAMON, SIHANDAL, BUTIK, EXSIST, HCIS, SMART, SIFU, SIPATUH, ESM, e-Payment, e-PPT (e-Pajak), dll.
  - tool TI: Jenkins, ArgoCD, Docker, Helm, OpenShift, GitLab, Nexus, SonarQube, Jira, K6, Katalon, Burp Suite, Dynatrace, Elastic, Kaseya, Dataedo, KNIME, SPSS, WebLogic, Google Workspace, draw.io
  - **8 proses bisnis belum didukung aplikasi**; masih Google Workspace (D2 hlm. 36)
  - landscape memisahkan aplikasi yang terintegrasi dan yang tidak terintegrasi dengan SMILE (hlm. 45)
- **Teknologi aplikasi** (D2 hlm. 37, slide 414; dicek visual):
  - **monolitik di WebLogic**; lapisan LB → Firewall → WAF/**API Security** → Web App/**API Management** → Business/Corporate/Enterprise Support Services
  - **DB: Oracle, MariaDB, Yugabyte, SQL Server, PostgreSQL**
  - proyeksi **70 juta peserta pada 2026**
- **Framework** (D2 hlm. 38):
  - frontend VueJS (16), BFF/backend NodeJS Express (21/19), Java (18), PHP, .NET (Ocelot), Golang, Flutter/Kotlin/Swift
  - **Oracle dipakai 30 aplikasi**, MariaDB 9, PostgreSQL 2, Yugabyte 2, SQL Server 1
- **Integrasi internal** (D2 hlm. 39, slide 417): API, Middleware/WS, dan **Direct-to-DB**. SMILE menjadi hub integrasi dan **berpotensi single point of failure**.
- **Integrasi eksternal** (D2 hlm. 40, slide 418; dicek visual):
  - **regulator:** **DUKCAPIL, SISKOP2MI, Mitra Pajakku**
  - external gateway: SMS, email
  - payment: **Bank, MotionPay, Xendit, Midtrans**
  - **Kanal Mitra, SiapKerja, Tanamduit, Jasnita, BSRE**
  - koneksi **Host-to-Host** (mayoritas ke SMILE, Bank) dan **API**
  - tidak seragam; **tiap aplikasi berintegrasi sendiri-sendiri, belum terpusat**
- **Infrastruktur** (D2 hlm. 50, slide 452; dicek visual):
  - IBB memuat APM, IT infra monitoring, ITSM, desktop management; **API Gateway** (sebagai komponen platform), app/DB/mail server, OS, container, hypervisor, bare metal; cloud storage (file server); SAN/NAS/object storage; enterprise backup, disk, tape; SD-WAN, SDN, VLAN; DC dan DRC
  - highlight: monitoring belum dipetakan ke aplikasi; **server on-premise** dengan virtualisasi dan container; off-site tape backup di Grha **tanpa restore test berkala**; **DC dan DRC colocation Tier III Uptime**
- **Organisasi data** (D2 hlm. 51, slide 555): di bawah **Deputi Bidang Manajemen Data & Analitik** ada Asdep **Data Governance**, **Interoperability & Processing**, **Data Analytics**, **Data Collaboration**, dipetakan ke COBIT APO01/02/03/09/14, MEA02, dll.
  - Sumber: Perdir 2/Perdir.04/012024.
  - Juga ada Deputi Arsitektur & Pengembangan TI dan Deputi Infrastruktur & Operasional TI, dengan Asdep Keamanan TI, Platform Teknologi & Keamanan, Operasional DC/DRC, PSI I–IV, User Services & Manajemen Kualitas.

---

## 4. Pemetaan ke 11 Knowledge Area DAMA-DMBOK v2

| KA DMBOK | D1 (kebijakan "seharusnya") | D2 (kondisi kini) | Penilaian / implikasi ESDP |
|---|---|---|---|
| 1. Data Governance | Struktur KPTI–UPTI–Unit Manajemen Data, kancab = data producer (Ps. 4–8, hlm. 7–10); acuan DMBOK (hlm. 6); ITMP; monev (Bab VII) | Asdep Data Governance ada (hlm. 51); tetapi **kewenangan perubahan elemen data tidak jelas** (hlm. 26); fungsi pengelola master data belum didefinisikan (hlm. 24) | Kebijakan kuat, **operating model belum lengkap**: peran data owner/steward per domain belum dirinci. Use case 3 (data governance) harus membuat RACI owner/steward/custodian per domain DBB |
| 2. Data Architecture | Ps. 13: DBB/domain/entitas, mapping ke bisnis & aplikasi, data flow, roadmap (hlm. 14–15) | DBB L0–L3 (hlm. 15–18); diagram kapabilitas (hlm. 20); DBB masih acuan ITMP 2019–2024 (hlm. 31) | **Dapat dipakai langsung** sebagai baseline. Gap: data flow per domain dan mapping entitas↔aplikasi (SoR mana untuk entitas apa) tidak ada di potongan D2 |
| 3. Data Modeling & Design | EDM oleh Unit Manajemen Data; PDM wajib sesuai Kamus Data & EDM (Ps. 14, hlm. 16–17); TSD memuat ERD (hlm. 67–68) | EDM 11 halaman, hanya 2 yang disertakan (hlm. 29–30); entitas belum tersistem | Minta EDM lengkap 1–11/11. Jadikan dasar **canonical data model / data contract** API |
| 4. Data Storage & Operations | Evaluasi teknologi DB, backup, archiving (Lamp. II.B, hlm. 89–92); DRP diuji (Ps. 42) | 5 jenis DB; Oracle 30 aplikasi; ADG standby; on-prem DC/DRC Tier III; **tape tanpa restore test** (hlm. 37–38, 50) | Heterogenitas DB memengaruhi pola CDC. Restore test belum berkala sehingga belum sesuai Ps. 42(2) |
| 5. Data Security | SMKI, CISO/ISM, kriptografi, **klasifikasi Biasa/Terbatas/Rahasia**, matriks CRUD, PAM, log, VA/pentest, **keamanan interoperabilitas Ps. 66** (hlm. 39–52, 93–107) | Lapisan WAF/API Security (hlm. 37); risiko data lake & DWH satu environment (hlm. 21); risiko kebocoran pertukaran eksternal (hlm. 21–22) | Kerangka kontrol interop sudah ada. Gap: klasifikasi 3 tingkat **belum dipetakan** ke kategori UU PDP (data pribadi umum/spesifik) dan **PP 71/2019 (data elektronik strategis/tinggi/rendah)**; tidak ada data masking/tokenisasi disebut |
| 6. Data Integration & Interoperability | Pertukaran eksternal **host-to-host: web service + SFTP**; teknologi lain wajib kajian keamanan (Ps. 66(3–4), hlm. 51); ETL/virtualization di DWBI (hlm. 77) | Talend, Qlik Replicate, ADG; API & SFTP (hlm. 21); internal API/Middleware/**Direct-to-DB**, SMILE hub (hlm. 39); eksternal H2H+API tidak seragam, **tidak terpusat** (hlm. 40) | **Inti studi.** Belum ada integration/API layer terpusat. Pola Direct-to-DB perlu di-eliminasi. Ps. 66 belum mengenal API gateway/OAuth2/mTLS/event streaming secara eksplisit, sehingga target arsitektur perlu dasar kebijakan (revisi Perdir atau juknis) |
| 7. Document & Content Mgmt | Pengendalian dokumen layanan TI (Ps. 30, hlm. 26–27); removable media (hlm. 95–97) | DBB memuat data arsip (JHT/JKK/JKM, PMI, naskah dinas) dan "Data Content Management System" (hlm. 18); unstructured data di lake belum operasional (hlm. 20) | Lemah. Dokumen klaim/KYC (unstructured) belum masuk platform. Di luar scope inti, tetapi relevan untuk e-claim |
| 8. Reference & Master Data | MD&DR: shared, ownership, **single master data repository terpusat**, wajib dipakai developer, review tahunan (Ps. 32; Lamp. II.A, hlm. 30, 87–89) | **Manual, tanpa MDM tool, tanpa SOP, tanpa fungsi pengelola**; big data tidak mengacu master data (hlm. 24); ilustrasi target MDM (hlm. 23) | **Gap terbesar kebijakan vs realita.** Penentuan batas SoR–MDM–ESDP–Cloudera adalah pertanyaan kunci TOR. Kandidat master: Peserta/TK (NIK), Pemberi Kerja/BU (NPP), Kantor, Mitra/Bank, Wilayah, Program |
| 9. DW & BI | Arsitektur referensi DWBI termasuk **ODS, MD&DR, cubes, big data platform** (Lamp. I.C, hlm. 75–80) | Cloudera untuk lake+DWH satu env; data mart; KNIME/Python; Tableau & QlikSense; 17 dashboard; antrean request ke MDT (hlm. 21–22) | Platform ada. Gap: **ODS tidak tampak di D2** padahal ada di referensi D1 dan di tender publik "Oracle ODS", sehingga perlu klarifikasi. Self-service belum (Ps. Lamp I.C prinsip d) |
| 10. Metadata | Glossary & kamus data 11 atribut (Lamp. I.B, hlm. 73–75); katalog data (Ps. 31) | **Dataedo (Oracle) + Apache Atlas (Cloudera)**, dua katalog terpisah; definisi tidak konsisten (hlm. 26) | Perlu katalog terpadu/federasi dan lineage end-to-end (sumber → Qlik/Talend → Cloudera → BI). Untuk data contract, metadata harus menjadi sumber skema |
| 11. Data Quality | 6 dimensi, CDE, formula, skala, laporan bulanan, evaluasi triwulanan (Lamp. I.D, hlm. 80–86) | Masalah duplikasi/inkonsistensi; DQ tool tanpa rekomendasi; perbaikan cabang lemah (hlm. 28); ada Dashboard Kualitas Data (hlm. 22) | Standar DQ **dapat dipakai langsung** sebagai DQ rule di data contract dan reconciliation interop (mis. konsistensi kanal vs SMILE, hlm. 84) |

---

## 5. Pemetaan ke deliverable TOR

| Deliverable TOR | Bahan yang dapat dipakai langsung | Kekurangan |
|---|---|---|
| Reviu existing architecture | D2 hlm. 20–22 (alur data), 35–45 (aplikasi & integrasi), 37–38 (teknologi), 50 (infra) | Slide yang hilang (353–555 tidak lengkap); tidak ada volumetri, SLA, jumlah interface |
| Lanskap data/integrasi/interoperabilitas | D2 hlm. 21 (Talend, Qlik Replicate, ADG, API, SFTP), 39–40 (diagram internal & eksternal) | Inventori interface (endpoint, frekuensi, format, volume, owner) belum ada; mitra K/L dalam diagram hanya Dukcapil, SISKOP2MI, Pajakku, SiapKerja |
| Gap analysis | D2 kolom "Kondisi saat ini / Risiko" (hlm. 21–28, 31) vs D1 pasal terkait (tabel §4) | Tidak ada skor maturity per KA DMBOK. Perlu asesmen tambahan |
| Target architecture (batas SoR–MDM–ESDP–Cloudera) | Komponen referensi DWBI D1 hlm. 77; prinsip MD&DR D1 hlm. 87–89; ilustrasi MDM D2 hlm. 23 | Belum ada definisi SoR per entitas, dan "ESDP" tidak disebut sama sekali di kedua dokumen |
| Interoperability framework | Ps. 66 (persetujuan akses, NDA, IP whitelist, web service, SFTP) D1 hlm. 50–51; isi minimum PKS Ps. 71 D1 hlm. 56; skema kolaborasi Ps. 69; klasifikasi data D1 hlm. 93–94; kriptografi hlm. 44–45; log hlm. 105–106; DQ formula hlm. 83–85; SLA insiden Ps. 37 hlm. 32–33; value data Ps. 72 | Belum ada standar API (REST/versioning/error), data contract/schema registry, SLA interface, monitoring, reconciliation, prosedur onboarding mitra teknis |
| UC1: kesiapan infra & platform ESDP | D2 hlm. 50 (on-prem, container OpenShift, DC/DRC Tier III); hlm. 21 (lake+DWH satu env) | Kapasitas Cloudera, versi CDP, node, sizing tidak ada di dokumen |
| UC2: API/pertukaran data | D2 hlm. 37, 40; D1 Ps. 66, 69–73 | Produk API management/gateway tidak dinamai |
| UC3: data governance | D1 seluruh Bab II, III, IV, Lamp. I–II; D2 hlm. 24, 26, 28, 51 | RACI steward per domain; status implementasi glossary/katalog |
| Transisi + roadmap 3 tahun | ITMP min. 3 tahun (Ps. 10, D1 hlm. 11–13); peralihan PKS 6 bulan (Ps. 78) | Roadmap ITMP 2025–2027 tidak disertakan |
| Rekomendasi teknologi | Kriteria evaluasi DB 10 butir (D1 hlm. 89); COTS harus kajian independen (D1 hlm. 64); Ps. 66(4) teknologi baru wajib kajian keamanan | Daftar lisensi/produk eksisting tidak ada di dokumen (lihat cross-check §7) |

---

## 6. Gap & inkonsistensi yang teridentifikasi

1. **MDM: kebijakan vs realita.** D1 mewajibkan satu master data repository terpusat dan wajib dipakai developer (hlm. 88). D2 menyatakan master data manual, tanpa tool, SOP, maupun fungsi (hlm. 24).
2. **Teknologi interoperabilitas.** Ps. 66 D1 hanya mengakui "host-to-host" berupa web service dan SFTP (hlm. 51). D2 menyebut API, Middleware/WS, **Direct-to-DB** (internal), serta platform kolaborasi eksternal "beragam" (hlm. 22, 39–40). Pola Direct-to-DB dan platform pihak ketiga perlu dicek kepatuhannya terhadap Ps. 66(4).
3. **API Management/Gateway.** Muncul sebagai komponen di D2 hlm. 37 (API Security, API Management) dan hlm. 50 (API Gateway), tetapi **produknya tidak disebut** dan tidak tampak di diagram integrasi (hlm. 39–40). Riset publik juga tidak menemukan tender API gateway; yang ada hanya "Keamanan API" 2024 dan Postman. Perlu konfirmasi apakah API management benar-benar ada atau hanya komponen konseptual.
4. **ODS.** D1 mencantumkan ODS sebagai komponen DWBI (hlm. 77). Tender publik 2023 menyebut "Oracle di Mesin Operational Data Store". D2 tidak menyebut ODS; yang disebut "Data Staging (stand by database)" via ADG (hlm. 21). Kemungkinan ODS = standby ADG (inferensi, perlu konfirmasi).
5. **Lake dan DWH satu environment Cloudera** (hlm. 21) bertentangan dengan prinsip pemisahan environment (D1 hlm. 104–105) dan klasifikasi Rahasia.
6. **Dua katalog metadata** (Dataedo untuk Oracle, Atlas untuk Cloudera) tanpa integrasi, padahal D1 Ps. 31 menghendaki katalog data terpusat berbasis metadata.
7. **Klasifikasi data 3 tingkat** (D1 hlm. 93–94) belum diselaraskan dengan UU PDP (yang dirujuk di konsiderans) dan kategori PP 71/2019. D1 menyatakan PDP diatur Perdir tersendiri (Ps. 3(4), hlm. 6), dan Perdir PDP itu belum kita miliki.
8. **Mitra K/L dalam diagram integrasi eksternal** (hlm. 40) hanya Dukcapil, SISKOP2MI, Pajakku, SiapKerja. **DJP, BPJS Kesehatan (e-PLKK), Satu Data/SPBE, Inpres 4/2025 DTSEN tidak tampak**, padahal secara publik ada pertukaran data DJP sejak 2022 dan integrasi e-PLKK dengan BPJS Kesehatan (go-live Des 2025). Kemungkinan lewat SFTP/bulk atau di luar cakupan diagram.
9. **Inpres 4/2025 DTSEN** menjadi dasar hukum D1 (hlm. 3), tetapi tidak ada mekanismenya di D2. Ini kewajiban baru yang relevan bagi ESDP.
10. **Dokumen D2 tidak lengkap:** EDM 4–11/11, Application Portfolio Assessment, rincian pain point, dan detail infrastruktur tidak disertakan.
11. **Kepemilikan platform DWBI ganda:** D1 menyebut Unit Manajemen Data "mengimplementasikan perangkat keras/lunak DWBI" sedangkan Unit Operasional TI "bertanggung jawab atas operasional dan keamanan" (hlm. 76–77). Ini batas peran yang perlu ditegaskan di target operating model ESDP.
12. **Restore test tape tidak berkala** (D2 hlm. 50) tidak sesuai kewajiban uji DRP berkala (D1 Ps. 42).

---

## 7. Cross-check dengan stack publik (riset tender sebelumnya)

| Komponen | Publik (tender) | D1/D2 | Status |
|---|---|---|---|
| Cloudera CDP (lake/DWH) | Ya (renewal 2024, infra 2023/2024/2025) | D2 hlm. 21 | **Terkonfirmasi** |
| Qlik Replicate (CDC) | Ya (2024) | D2 hlm. 21 | **Terkonfirmasi** |
| Talend (ETL/DQ) | Ya (2023, 2024) | D2 hlm. 21 | **Terkonfirmasi** (ETL; DQ tool tidak dinamai) |
| Dataedo | Ya (2024) | D2 hlm. 26 (untuk Oracle) | **Terkonfirmasi** |
| Apache Atlas | Tidak ada di tender | D2 hlm. 26 | **Baru** (bawaan CDP) |
| Oracle (+ODS/ADG) | Ya (ATS 2023–24, ODS 2024, SRS 2026) | D2 hlm. 21 (ADG), 37–38 (30 aplikasi) | **Terkonfirmasi**; ODS perlu klarifikasi |
| MariaDB | Ya (2024) | D2 hlm. 21, 37–38 | **Terkonfirmasi** |
| YugabyteDB | Rencana 2025 | D2 hlm. 37–38 (2 aplikasi) | **Terkonfirmasi** |
| SQL Server, PostgreSQL | Tidak ditemukan | D2 hlm. 37–38 | **Baru** |
| KNIME | Ya (2024) | D2 hlm. 22, 35 | **Terkonfirmasi** |
| Tableau | Ya (rencana 2025) | D2 hlm. 22 | **Terkonfirmasi** |
| QlikSense | Diduga (dashboard tool tak dinamai) | D2 hlm. 22 | **Terkonfirmasi** |
| SPSS, Python | Tidak | D2 hlm. 22, 35 | **Baru** |
| OpenShift/Docker/Helm/ArgoCD/Jenkins/GitLab/Nexus/SonarQube | Sebagian (container 2023/2025, GitLab) | D2 hlm. 35, 39, 45 | **Terkonfirmasi**: container platform = OpenShift |
| WebLogic | Tidak | D2 hlm. 37 | **Baru** (app server monolit) |
| Elastic, Dynatrace | Elastic ya; Dynatrace tidak | D2 hlm. 35 | Elastic terkonfirmasi; Dynatrace baru |
| MDM tool | Tidak ditemukan | D2 hlm. 24: **tidak ada** | **Terkonfirmasi tidak ada** |
| API gateway/management | Tidak ditemukan (hanya API Security & Postman) | D2 hlm. 37, 50 sebagai komponen tanpa produk | **Ambigu**, perlu konfirmasi |
| ESB | Tidak ditemukan | D2: "Middleware/WS" (hlm. 39); "ESM" = Enterprise Service Management, bukan ESB | **Tidak ada ESB eksplisit** |
| DC/DRC on-prem | Ya | D2 hlm. 50: colocation Tier III | **Terkonfirmasi** |
| SMILE (core) | Disebut di berita BPK 2026 | D2 hlm. 35, 39 | **Terkonfirmasi** |

---

## 8. Apa yang dapat dipakai langsung (reusable) dengan halaman

- Kerangka DBB L0–L3 dan daftar entitas: **D2 hlm. 15–18** → katalog domain ESDP / domain ownership.
- Diagram kapabilitas data as-is: **D2 hlm. 20** → baseline "as-is" di laporan.
- Tabel kondisi & risiko per komponen: **D2 hlm. 21–22, 24, 26, 28, 31** → input gap analysis.
- Diagram integrasi internal & eksternal: **D2 hlm. 39–40** → baseline integration landscape (perlu dilengkapi).
- Teknologi & framework aplikasi: **D2 hlm. 37–38**.
- IBB infrastruktur: **D2 hlm. 50**.
- Struktur organisasi data & COBIT: **D2 hlm. 51**.
- Mandat Unit Manajemen Data: **D1 hlm. 9 (Ps. 8(3))**.
- Arsitektur referensi DWBI: **D1 hlm. 77** → "kebijakan sudah mengizinkan" komponen ODS/MD&DR/lake.
- Prinsip & arsitektur MD&DR: **D1 hlm. 87–89** → dasar desain MDM.
- Standar metadata/kamus data 11 atribut: **D1 hlm. 74–75** → dasar data contract.
- Standar DQ (dimensi, formula, skala): **D1 hlm. 80–86** → aturan DQ dan rekonsiliasi interop.
- Keamanan interoperabilitas: **D1 hlm. 50–51 (Ps. 66)**; klasifikasi: **D1 hlm. 93–94**; kriptografi: **D1 hlm. 44–45**; log: **D1 hlm. 105–106**.
- Kolaborasi data, skema, isi PKS, value data: **D1 hlm. 53–58 (Ps. 68–73)** → bahan onboarding mitra pada interoperability framework.
- SLA insiden: **D1 hlm. 32–33** → acuan SLA operasional interface.
- Ketentuan ITMP 3 tahun: **D1 hlm. 11–13** → roadmap 3 tahun selaras ITMP.

---

## 9. Pertanyaan untuk BPJS Ketenagakerjaan

**Dokumen & ruang lingkup**
1. Mohon dokumen EA As-Is lengkap (slide 1–555), termasuk EDM 1–11/11, Application Portfolio Assessment (D.4), rincian pain point (A.x–D.x), dan apakah ada EA To-Be / ITMP 2025–2027.
2. Mohon Perdir PDP (Ps. 3(4) D1) dan Perdir 2/Perdir.04/012024 (struktur organisasi), serta SOP/juknis turunan Perdir 4/2025 (katalog data, MD&DR, DQ, klasifikasi).
3. Apakah istilah "ESDP" sudah memiliki definisi resmi di ITMP atau dokumen lain?

**Integrasi & interoperabilitas**
4. Apakah API Management/API Gateway (D2 hlm. 37, 50) sudah ada? Produk apa, cakupannya untuk internal/eksternal, dan bagaimana hubungannya dengan tender "Keamanan API" 2024?
5. Apa yang dimaksud "Middleware/WS" (D2 hlm. 39)? Produk apa (mis. WebLogic/OSB)? Berapa jumlah interface Direct-to-DB?
6. Mohon inventori interface eksternal: mitra (termasuk DJP, BPJS Kesehatan/e-PLKK, Kemnaker selain SiapKerja, SISKOP2MI, Satu Data/DTSEN), pola (H2H/API/SFTP), frekuensi, volume, format, SLA, owner, status PKS (sudah diadendum sesuai Ps. 78?).
7. "Platform eksternal yang beragam" untuk data collaboration (D2 hlm. 22): platform apa saja dan bagaimana kontrolnya terhadap Ps. 66?
8. Bagaimana mekanisme pemenuhan Inpres 4/2025 DTSEN saat ini?

**Data platform**
9. Konfigurasi Cloudera: versi CDP, jumlah node, kapasitas, komponen (Hive/Impala/Spark/Kafka/NiFi/Atlas/Ranger), dan rencana pemisahan lake vs DWH.
10. Apakah "Operational Data Store" (tender Oracle ODS) sama dengan Data Staging/ADG standby di D2 hlm. 21? Apa perannya terhadap SMILE?
11. Cakupan Qlik Replicate vs Talend (sumber mana, CDC vs batch) dan latensinya. Apakah ada event streaming (Kafka) dan bagaimana kaitannya dengan rencana "CDC dan data streaming" 2025?
12. Tool DQ "automated" yang dimaksud (D2 hlm. 28): Talend DQ atau lainnya?

**Master data & governance**
13. Entitas apa yang dianggap master data saat ini (Peserta/NIK, PK/BU/NPP, Kantor, Mitra, Referensi wilayah/kode)? Di mana SoR masing-masing (SMILE?) dan siapa owner/steward-nya?
14. Apakah business glossary & kamus data sudah terbit/termutakhir? Di tool apa? Berapa CDE dan skor DQ terakhir?
15. Bagaimana peran Asdep Data Governance, Interoperability & Processing, dan Data Collaboration dibagi dalam praktik? Siapa pemilik platform ESDP (Unit Manajemen Data vs Unit Operasional TI)?

**Keamanan**
16. Bagaimana pemetaan klasifikasi Biasa/Terbatas/Rahasia ke kategori data pribadi UU PDP dan PP 71/2019? Apakah ada masking/tokenisasi untuk data di lake dan pertukaran?
17. Apakah ada kebijakan penggunaan cloud? D2 hanya menyebut "cloud storage" sebagai file server (hlm. 50), dan BPM yang direncanakan berbasis cloud.

---

## 10. Catatan keterbatasan analisis
- D1 dibaca dari teks OCR. Isi yang dikutip konsisten, tetapi tabel contoh formula DQ (hlm. 84) dan beberapa kata mungkin tidak sempurna.
- D2: teks diekstrak dari PowerPoint. Beberapa diagram (hlm. 20, 37, 40, 50) diverifikasi secara visual; diagram lain (DBB, EDM, value chain) hanya dibaca dari teks sehingga relasi panahnya tidak terverifikasi.
- Tidak ada informasi yang ditambahkan di luar kedua dokumen kecuali pada §7 (cross-check riset publik) dan bagian yang ditandai "inferensi".


---

## 11. Sumber tambahan (D3, D4, D5, R1) — pembaruan 31 Okt 2026

Sumber berikut diterima setelah analisis D1–D2 dan dipakai di berkas 11–16 dengan kode singkat di bawah. Status semuanya **dokumen terbatas**.

| Kode | Sumber | Isi pokok | Konvensi sitasi |
|---|---|---|---|
| **D3** | Tujuh tangkapan layar paparan BPJS (D3-01 s.d. D3-07) | D3-01 status RAG kapabilitas platform dan komponen DC/DRC; D3-02 alur POC MDM (sumber JMO/POM/Lapak Asik dan SMILE Pendaftaran/Klaim → pembobotan dan pengecekan kualitas → golden record → tabel data mart); D3-03 pola arsitektur MDM (Data Stewardship, Pencocokan, 360° Database, Zona Penyebaran Data via API); D3-04 dan D3-05 model konseptual Kepesertaan (Gambar 17, 13 entitas berdefinisi); D3-06 model konseptual Pelayanan/Klaim (Gambar 27); D3-07 lapisan arsitektur target | **D3-0n** (nomor gambar) |
| **D4** | Dokumen kebijakan/teknis API Data Hub | Data Hub ditempatkan di DMZ untuk pertukaran B2B, REST/JSON, melalui API Management (3scale); kelayakan mitra berdasarkan PKS dan dokumen teknis | **D4** [halaman tidak bernomor; perlu konfirmasi rujukan bagian] |
| **D5** | Dokumen teknis API pertukaran data BPJS TK–Kemnaker (12 hlm. PDF) | Endpoint dua arah (BPJS → Kemnaker per NIK dan NPP; Kemnaker → BPJS per NIK dan NIB), alur Client Request → Key Validation → proses → respons via API Management, kueri berbasis DB link, salinan lokal tabel mitra, berkas konfigurasi | **D5 hlm. n** |
| **R1** | Ekstraksi rekaman diskusi dengan tim BPJS (sesi pagi dan sesi siang) | Klarifikasi praktik: kunci identitas, CDC Qlik, Talend, freshness per jam, SCD, Kudu/Parquet, pemisahan lapis berbasis skema, lineage belum ada, masking sebagian, SFTP AES-256, status DRC, POC MDM | **R1 [P hh:mm]** = sesi pagi; **R1 [S hh:mm]** = sesi siang |

**Catatan penanganan.**
- D5 memuat berkas konfigurasi dengan kredensial. Temuan dicatat hanya sebagai "kredensial tersimpan dalam teks biasa pada dokumen konfigurasi"; nilai kredensial, alamat jaringan, nama host, dan nama skema/akun **tidak** disalin ke berkas analisis mana pun.
- D3 berupa gambar tanpa nomor halaman; status RAG dibaca dari warna titik. Butir tanpa titik (mis. Restore Testing, Metadata Management, Data Profiling pada D3-01) dicatat sebagai "status tidak ditandai".
- **Konflik sumber yang harus dikonfirmasi:** D3-01 menyatakan DRC aktif dengan komponen platform (20 node), sedangkan R1 [S 00:35–00:37] menyatakan belum ada komponen Cloudera di DRC [perlu konfirmasi].
- R1 adalah pernyataan lisan; bila bertentangan dengan dokumen, keduanya dicantumkan dan ditandai [perlu konfirmasi].
