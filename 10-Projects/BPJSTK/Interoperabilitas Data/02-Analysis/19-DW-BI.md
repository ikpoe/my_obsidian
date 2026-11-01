---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Data Warehouse & Business Intelligence
sesi: 9
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.3, 5.4, 6]
status: draft-diskusi
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, D3 status kapabilitas (D3-01), D4 Q&A WA 31 Okt 2026 (kebijakan Data Hub), D5 dokumen teknis API Kemnaker (RESTRICTED, hanya temuan), R1 rekaman diskusi 27 Okt 2026, 11-Master-Data.md, 12-Integrasi.md, 13-Arsitektur-Data.md, 14-Pemodelan-Data.md, 15-Metadata-Kualitas.md, 16-Keamanan-Data.md, 17-Tata-Kelola-Data.md, 18-Penyimpanan-Dokumen-Konten.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 78 = cetak '- 77 -')."
---

# Sesi 9 — Data Warehouse & Business Intelligence

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI. **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (as-is). **D3** = slide BPJS (D3-01 status RAG kapabilitas). **D4** = Q&A WhatsApp 31 Okt 2026. **D5** = dokumen teknis API Kemnaker (RESTRICTED; hanya temuan). **R1** = ekstraksi rekaman 27 Okt 2026, dirujuk [P hh:mm] / [S hh:mm]. "hlm." = halaman PDF.

> **Batas ruang lingkup.** Sama dengan sesi 8: **hanya yang dicakup ESDP** — lapis EDW (silver) dan data mart (gold), lapisan semantik, alat BI/reporting/dashboard **sejauh mengonsumsi data ESDP**, self-service analytics, definisi KPI/metrik, rasionalisasi laporan, freshness laporan, akses BI per klasifikasi, dan jalur downstream ke pengguna (BI internal vs serving B2B Data Hub). **Tidak dinilai:** laporan operasional yang dibangkitkan langsung dari aplikasi sumber (SMILE, aplikasi dashboard mandiri seperti Dashboard Investasi/Dashboard BUMN di D2 hlm. 35), sistem aktuaria/SPSS, dan platform kolaborasi milik mitra. Komponen itu hanya disebut sebagai antarmuka.

> **Kerangka sesi.** Opsi B di atas Cloudera CDP; medallion staging/bronze → silver (DWH) → gold (serving) ([[13-Arsitektur-Data]]). Silver memakai Data Vault 2.0 untuk domain inti; gold memakai Kimball star schema dengan conformed dimension dari MDM ([[14-Pemodelan-Data]]). Shared dimension tervalidasi: **Pemberi Kerja** pada grain UnitPemberiKerja; **Individu** dan **Peserta** sebagai dimensi terpisah; **Program** dan **Kanal** masih belum ada. CDE dan kualitas dari [[15-Metadata-Kualitas]]; kontrol per klasifikasi dari [[16-Keamanan-Data]]; owner/steward dari [[17-Tata-Kelola-Data]]; parameter freshness dan penyimpanan dari [[18-Penyimpanan-Dokumen-Konten]]. Downstream internal (BI) dan downstream eksternal (Data Hub di DMZ, B2B, 3scale/SFTP; D4) sama-sama membaca dari gold. Blueprint menetapkan parameter, bukan nilai.

## 1. Ringkasan
- **DW berjalan, tetapi tanpa model gold yang dikelola.** Data mart sebagian besar berupa tabel Kudu/Parquet hasil job Talend di Cloudera, dibentuk per kebutuhan bisnis (R1 [S 00:30:25–00:31:48]); ada banyak cube untuk aplikasi finance (R1 [S 00:21:36–00:21:46]). Pola model, SCD, dan bus matrix tidak terdokumentasi ([[14-Pemodelan-Data]]).
- **Alat BI ganda dan tanpa lapisan semantik.** Konsumsi melalui Tableau dan QlikSense; analitik dengan Python dan KNIME; dashboard yang disebut: Pengadaan, Kualitas Data, Keagenan, Command Center, Pelayanan, Anggaran, Pembayaran (D2 hlm. 22). Definisi KPI/metrik terpusat tidak ditemukan; Data Catalog dan Lineage merah (D3-01).
- **Permintaan analitik terpusat dan lambat.** Unit bisnis mengirim request ke tim data, "sehingga membutuhkan waktu yang cukup lama" (D2 hlm. 22). D1 sudah mengatur mekanisme kebutuhan bisnis dan portofolio BI (Lampiran I C.3–C.4, hlm. 78–79), tetapi self-service belum ada.
- **Jalur downstream bercampur.** Sebagian core system mengonsumsi langsung dari environment big data (R1 [S 00:21:48–00:22:04]); data collaboration memakai platform eksternal yang beragam per mitra (D2 hlm. 22); Data Hub belum dibangun (D4; D3-01 API Management merah).
- **Kebutuhan gold jelas.** Forecasting, risiko likuiditas (maturity iuran/klaim untuk investasi/treasury), dashboard star schema, dan reporting relasional (R1 [S 00:43:04–00:45:46; P 00:42:52–00:43:31]); dimensi waktu krusial (R1 [P 01:32:23–01:32:35]).
- **Skor usulan: DW & BI ±1,5** (13 aspek, rentang 1–2).
- **Rekomendasi (usulan, berbasis perbandingan):** **Opsi B — governed self-service.** Hub data membangun gold tersertifikasi (conformed dimension + fact per domain) dan **lapisan metrik/semantik terpusat yang netral alat (S2)**; unit bisnis membuat dashboard sendiri di atas dataset tersertifikasi; BI internal dan Data Hub B2B membaca gold yang sama dengan jalur akses terpisah.

---

## 2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| EDW (silver) | Tabel CDC Kudu (per jam) → Parquet (≥1–2 jam) | R1 [S 00:17:55–00:19:21; S 00:29:19–00:30:41] | Lapis integrasi terpola (Vault) belum ada |
| Data mart (gold) | Kudu/Parquet hasil Talend, per kebutuhan bisnis; DWH dikategorikan ke data mart per unit | R1 [S 00:31:16–00:31:48]; D2 hlm. 21 | Inventaris mart tidak diterima [perlu konfirmasi] |
| Cube | Banyak cube untuk aplikasi finance | R1 [S 00:21:36–00:21:46] | Alat cube & pemilik tidak diketahui |
| Conformed dimension | Pemberi Kerja (UnitPemberiKerja), Individu, Peserta tervalidasi; Program dan Kanal belum ada | [[14-Pemodelan-Data]]; D3-04/05 | Tanpa Program/Kanal, KPI per program dan kanal tidak konsisten |
| Alat BI | Tableau dan QlikSense | D2 hlm. 22 | Dua alat, kemungkinan definisi ganda |
| Analitik lanjut | Python, KNIME; predictive per use case; prescriptive belum | D2 hlm. 22 | — |
| Portofolio dashboard | Pengadaan, Kualitas Data, Keagenan, Command Center, Pelayanan, Anggaran, Pembayaran | D2 hlm. 22 | Jumlah, pemakaian, dan duplikasi tidak diketahui |
| Lapisan semantik / KPI | Tidak ditemukan | D2 hlm. 20 (Metadata, Data Catalog bertanda belum operasional); D3-01 | — |
| Permintaan analitik | Request ke tim data; lama | D2 hlm. 22 | Kapasitas tim menjadi bottleneck |
| Freshness | Paling cepat 1 jam; streaming belum ada | R1 [S 00:03:54–00:04:47] | SLA per laporan tidak ada ([[18-Penyimpanan-Dokumen-Konten]] PS-6) |
| Akses BI | Tidak ada bukti pemetaan klasifikasi ke izin Tableau/QlikSense | [[16-Keamanan-Data]] | [perlu konfirmasi] |
| Downstream | Core system membaca langsung dari big data (sedikit, mis. report); kolaborasi via platform eksternal beragam; Data Hub belum ada | R1 [S 00:21:48–00:22:42]; D2 hlm. 22; D4 | Kasus Kemnaker (D5): akses DB langsung, bukan produk gold |

## 3. Kondisi yang seharusnya (D1)
- **Ps. 16 (hlm. 18):** UMD mengelola DWBI/big data: arsitektur, kebutuhan Unit Bisnis, serta pengembangan dan operasional, berkoordinasi dengan Unit Pengembangan TI dan Unit Operasional TI.
- **Lampiran I C.2 (hlm. 76–78):** arsitektur DWBI mencakup sumber hingga presentasi/visualisasi; pertimbangan kinerja & ketersediaan, keamanan & privasi, interoperabilitas.
- **Lampiran I C.3 (hlm. 78):** kebutuhan bisnis disampaikan Direksi/Unit Bisnis ke UMD; UMD memprioritaskan berdasar keselarasan strategi, urgensi regulasi, kebutuhan spesifik, **kesiapan sumber data**; hasilnya menjadi program kerja tahunan.
- **Lampiran I C.4 (hlm. 78–80):** descriptive/diagnostic melalui pengembangan DW dan data mart, populasi DW, **portofolio BI** (identifikasi kelompok pengguna, alat sesuai kelompok), dan pemeliharaan produk data; predictive/prescriptive melalui siklus akuisisi → hipotesis → model → deployment & monitoring.
- **Lampiran I D (hlm. 80–81):** metrik kualitas data disusun UMD bersama fungsi perencanaan strategis dan manajemen risiko.
- **Ps. 31 (hlm. 29):** Katalog Data dengan keamanan berbasis klasifikasi. **Lampiran III A.2 (hlm. 93–94):** kontrol akses per klasifikasi.
- **Yang tidak diatur D1:** lapisan semantik, definisi KPI baku dan pemiliknya, sertifikasi dataset/laporan, self-service, rasionalisasi laporan, SLA freshness laporan.

## 4. Tabel penilaian — DW & BI (lingkup ESDP)

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal.

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2/D3/R1/D4/D5) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| DB-1. Lapis EDW (silver) | Lapis integrasi historis terpola, terhubung kunci MDM | Lampiran I C.4.b.1–2 (hlm. 78–79) | Kudu→Parquet berjalan per jam (R1); pola model tidak terdokumentasi | Pola & standar | 2 |
| DB-2. Portofolio data mart & conformed dimension | Bus matrix; fact per proses; dimensi bersama | Lampiran I C.4.b (hlm. 79) | Mart per kebutuhan (R1); 3 dimensi tervalidasi, Program & Kanal belum ([[14-Pemodelan-Data]]) | Bus matrix, 2 dimensi | 2 |
| DB-3. Lapisan semantik | Satu definisi measure/dimensi dipakai semua alat | Tidak ada | Tidak ditemukan; dua alat BI (D2 hlm. 22) | Lapisan semantik | 1 |
| DB-4. Definisi & kepemilikan KPI/metrik | Katalog KPI: definisi, rumus, grain, owner, CDE terkait | Metrik kualitas (hlm. 80), bukan KPI bisnis | Tidak ada katalog KPI; Data Catalog merah (D3-01) | Katalog KPI & owner | 1 |
| DB-5. Standar alat BI | Alat ditetapkan per kelompok pengguna | Lampiran I C.4.b.3.b (hlm. 79) | Tableau + QlikSense + cube finance (D2 hlm. 22; R1) | Rasionalisasi alat | 2 |
| DB-6. Portofolio & rasionalisasi laporan | Inventaris, pemakaian, pemilik; penghapusan duplikat | Portofolio BI (hlm. 79) | Daftar dashboard ada (D2 hlm. 22); inventaris/pemakaian tidak ada | Inventaris & metrik pemakaian | 2 (batas kebijakan) |
| DB-7. Manajemen permintaan analitik | Intake, prioritas, SLA, program kerja | Lampiran I C.3 (hlm. 78) | Request ke tim data, lama (D2 hlm. 22) | SLA & transparansi | 2 |
| DB-8. Self-service analytics | Dataset tersertifikasi, ruang kerja terkelola, pelatihan | Tidak ada | Tidak ada; bergantung tim data (D2 hlm. 22) | Model self-service | 1 |
| DB-9. Freshness laporan | Target freshness per laporan; indikator di dashboard | Tidak ada | 1–2 jam dalam praktik (R1); tanpa SLA | SLA & indikator | 2 |
| DB-10. Akses BI per klasifikasi | Izin Tableau/QlikSense mengikuti klasifikasi & tag Ranger; row-level security | Ps. 31(2)c; Lampiran III A.2 | Tidak ada bukti ([[16-Keamanan-Data]]) | Pemetaan izin | 1 [perlu konfirmasi] |
| DB-11. Sertifikasi kualitas dataset/laporan | Badge tersertifikasi bila CDE lolos ambang | Lampiran I D (hlm. 80–81) | Dashboard Kualitas Data ada (D2 hlm. 22); tidak ditautkan ke laporan | Tautan CDE → laporan | 1 |
| DB-12. Pemisahan jalur downstream | BI internal dan Data Hub B2B membaca gold; aplikasi tidak membaca silver langsung | Ps. 66 (hlm. 50–51) | Core system baca langsung (R1); platform kolaborasi beragam (D2 hlm. 22); Data Hub belum ada (D4); Kemnaker via DB (D5) | Jalur baku | 1 |
| DB-13. Analitik lanjut | Siklus model terkelola, deployment & monitoring | Lampiran I C.4.c (hlm. 79–80) | Python/KNIME; predictive per use case; prescriptive belum (D2 hlm. 22) | MLOps & dataset fitur | 2 |

**Rata-rata DW & BI: ±1,5** (20/13).

## 5. Opsi model operasi BI

| Opsi | Deskripsi |
|---|---|
| A. Tim BI terpusat | Tim data membangun semua mart, dashboard, dan laporan atas permintaan |
| B. Governed self-service | Hub membangun gold tersertifikasi dan lapisan metrik; unit bisnis membuat dashboard sendiri di atas dataset tersertifikasi; hub menyertifikasi laporan strategis |
| C. Mart domain terfederasi | Tiap domain membangun dan mengoperasikan mart dan BI-nya sendiri; hub hanya standar |

**Perbandingan (++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria | A | B | C |
|---|---|---|---|
| Menghilangkan bottleneck request (D2 hlm. 22) | −− | ++ | ++ |
| Konsistensi KPI lintas unit | ++ | + | −− |
| Kesesuaian D1 (UMD mengelola DWBI, Ps. 16) | ++ | + | − |
| Kesesuaian model tata kelola federasi ([[17-Tata-Kelola-Data]]) | 0 | ++ | + |
| Kesiapan saat ini (owner/steward belum ada, katalog merah) | + | 0 (bertahap) | −− |
| Risiko akses data Rahasia/PDP | + | + (dengan dataset tersertifikasi) | − |
| Skalabilitas ke 70 juta peserta dan banyak unit | − | + | + |

**Rekomendasi: Opsi B, bertahap.** Opsi A adalah kondisi sekarang dan penyebab antrean. Opsi C memecah definisi KPI saat dimensi Program dan Kanal belum ada. Opsi B mempertahankan peran UMD (Ps. 16) sambil memindahkan pembuatan dashboard ke pengguna. Tahun 1: gold tersertifikasi untuk 2–3 domain pilot (selaras pilot tata kelola) dan katalog KPI; Tahun 2: self-service terbuka per kelompok pengguna.

**Opsi lapisan semantik**

| Opsi | Deskripsi | Plus | Minus |
|---|---|---|---|
| S1. Di dalam tiap alat BI | Model data Tableau dan QlikSense masing-masing | Cepat; tanpa komponen baru | Definisi ganda; terikat alat |
| S2. Lapisan metrik terpusat netral alat | View/tabel gold tersertifikasi + definisi metrik di katalog (Dataedo) sebagai sumber tunggal; bisa ditingkatkan ke alat semantik khusus | Satu definisi untuk BI, aplikasi, dan Data Hub; selaras katalog | Butuh disiplin dan pemilik metrik |
| S3. Cube OLAP terpusat | Perluasan cube finance ke domain lain | Kinerja agregat baik; sudah dikenal tim finance | Kaku; sulit untuk self-service dan B2B |

**Rekomendasi semantik: S2.** Data Hub B2B dan BI internal harus menghitung metrik yang sama; hanya S2 yang tidak terikat alat. Cube finance eksisting dipertahankan sementara dan dipetakan ke definisi S2 [perlu konfirmasi alat cube].

## 6. Arsitektur konsumsi (downstream)

| Jalur | Membaca dari | Kontrol | Catatan |
|---|---|---|---|
| BI internal (Tableau/QlikSense) | Gold tersertifikasi via lapisan S2 | Ranger tag + izin alat BI per klasifikasi | Self-service di ruang kerja terkelola |
| Analitik lanjut (Python/KNIME) | Silver curated + gold | Ruang kerja terpisah, data Rahasia dimasking | Fitur hasil model kembali ke gold |
| Aplikasi internal (core system yang butuh report) | Gold via API internal | Kontrak data | Hentikan pembacaan silver langsung |
| Data Hub B2B (mitra) | Gold produk data per PKS → serving store DMZ → 3scale/SFTP | PKS + dokumen teknis (D4); [[17-Tata-Kelola-Data]] §9 | Menggantikan akses DB langsung (D5) |

## 7. Parameter wajib (tanpa nilai)

| Kelompok | Parameter | Pemilik (usulan) |
|---|---|---|
| KPI/metrik | Definisi, rumus, grain, dimensi yang diizinkan, owner, CDE sumber, frekuensi | Data owner domain; hub memelihara katalog |
| Sertifikasi | Kriteria dataset/laporan tersertifikasi; masa berlaku; ambang CDE ([[15-Metadata-Kualitas]]) | Hub + steward |
| Freshness & kinerja | Target freshness per laporan/mart; target waktu respons dashboard; jendela refresh ([[18-Penyimpanan-Dokumen-Konten]] §6) | Owner produk data |
| Permintaan | Kategori permintaan; SLA respons dan penyelesaian; kriteria prioritas (D1 hlm. 78) | UMD |
| Rasionalisasi | Ambang pemakaian minimum; periode reviu; kriteria penghapusan/penggabungan laporan | Hub + owner |
| Akses | Pemetaan klasifikasi → peran alat BI; aturan row-level; aturan ekspor/unduh | OKI + owner |
| Self-service | Kelompok pengguna; kuota sumber daya; syarat pelatihan; aturan publikasi | Hub |

## 8. Rasionalisasi laporan
Langkah: (1) inventaris dashboard/laporan/cube di Tableau, QlikSense, dan cube finance; (2) kumpulkan metrik pemakaian dan pemilik; (3) petakan ke KPI katalog dan domain DBB; (4) klasifikasikan **pertahankan / gabungkan / pindah ke self-service / pensiunkan**; (5) migrasi laporan strategis ke dataset tersertifikasi; (6) reviu berkala sesuai parameter. Laporan operasional aplikasi sumber di luar lingkup.

## 9. Batas peran

| Peran | Lingkup |
|---|---|
| Hub data (UMD) | Gold tersertifikasi, lapisan S2, katalog KPI, prioritas permintaan (Ps. 16) |
| Data owner domain | Definisi KPI dan akses domain |
| Steward | Kualitas CDE di balik KPI, sertifikasi |
| Pengguna self-service | Dashboard di ruang kerja terkelola, tanpa publikasi strategis tanpa sertifikasi |
| Unit Operasional TI / tim platform | Kinerja klaster dan server BI, refresh |
| OKI | Kebijakan akses dan ekspor |
| Pemilik aplikasi sumber | Di luar lingkup (laporan operasional) |

## 10. Keterkaitan dengan Blueprint
- **3.1–3.3:** bagian 2–4. **3.4 / 5.3–5.4:** bagian 5–9.
- **6 (roadmap) — usulan tema "Satu angka untuk semua pengguna":**
  - **Tahun 1 — Fondasi:** inventaris laporan; katalog KPI untuk domain pilot; bangun dimensi Program dan Kanal; gold tersertifikasi pertama; pemetaan akses BI per klasifikasi; target freshness laporan prioritas.
  - **Tahun 2 — Layanan:** governed self-service per kelompok pengguna; lapisan S2 dipakai Tableau, QlikSense, dan Data Hub; rasionalisasi gelombang pertama; hentikan pembacaan silver langsung oleh aplikasi.
  - **Tahun 3 — Optimasi:** keputusan konsolidasi alat BI; analitik lanjut (forecasting, risiko likuiditas) di atas gold; metrik pemakaian dan kualitas sebagai dasar reviu portofolio.

## 11. Pertanyaan terbuka untuk BPJS
1. Berapa jumlah dashboard/laporan dan cube yang membaca dari Cloudera, dan alat mana yang dipakai masing-masing?
2. Apakah ada definisi KPI baku (mis. peserta aktif, iuran terkumpul, klaim terbayar) dan siapa pemiliknya?
3. Bagaimana izin Tableau/QlikSense diberikan, dan apakah mengikuti klasifikasi data?
4. Aplikasi mana yang membaca langsung dari environment big data?
5. Mengapa ada dua alat BI, dan apakah ada rencana konsolidasi?

## 12. Poin diskusi (ikpu)
- [ ] Sepakati batas lingkup: BI yang mengonsumsi ESDP; laporan aplikasi sumber di luar.
- [ ] Sepakati Opsi B (governed self-service) dan lapisan semantik S2.
- [ ] Prioritaskan dimensi Program dan Kanal sebagai prasyarat KPI konsisten.
- [ ] Mulai katalog KPI dan inventaris laporan sebagai quick win Tahun 1.
- [ ] Sepakati pemisahan jalur downstream: BI internal dan Data Hub B2B sama-sama dari gold; aplikasi tidak membaca silver.
- [ ] Validasi DB-10 (akses BI per klasifikasi) bersama OKI.

## Penutup — 9 sesi asesmen selesai
Sesi ini menutup sembilan sesi asesmen (file 11–19). Rata-rata per file: Master Data ±1,9; Integrasi upstream ±1,9 / downstream ±1,8; Arsitektur Data ±1,8; Pemodelan Data ±1,6; Metadata ±1,4 / Kualitas Data ±1,6; Keamanan Data ±1,6; Tata Kelola Data ±1,4; Penyimpanan + Dokumen & Konten ±1,5; DW & BI ±1,5. Rata-rata sederhana lintas sesi ±1,6 (nilai ganda di sesi 12 dan 15 dirata-ratakan dulu per file). Polanya konsisten: kebijakan D1 cukup lengkap, tetapi peran, standar operasional, dan alat penegakan belum berjalan. Karena itu tahun pertama roadmap di semua sesi bertumpu pada fondasi yang sama: tata kelola federasi, katalog, dan klasifikasi.

| Sesi | File | Rata-rata |
|---|---|---|
| 1 | 11-Master-Data | ±1,9 |
| 2 | 12-Integrasi | ±1,9 (up) / ±1,8 (down) |
| 3 | 13-Arsitektur-Data | ±1,8 |
| 4 | 14-Pemodelan-Data | ±1,6 |
| 5 | 15-Metadata-Kualitas | ±1,4 / ±1,6 |
| 6 | 16-Keamanan-Data | ±1,6 |
| 7 | 17-Tata-Kelola-Data | ±1,4 |
| 8 | 18-Penyimpanan-Dokumen-Konten | ±1,5 |
| 9 | 19-DW-BI | ±1,5 |

## Lampiran — Deskripsi diagram (gaya Excalidraw)
Kiri ke kanan: **Silver (Data Vault, Kudu/Parquet)** → **Gold (star schema: fact per domain; dimensi Pemberi Kerja/UnitPemberiKerja, Individu, Peserta, Waktu; Program dan Kanal bertanda merah "belum ada")** → pita vertikal **Lapisan metrik S2 (katalog KPI di Dataedo)**. Dari pita S2 bercabang empat panah: **Tableau / QlikSense (self-service + laporan tersertifikasi)**, **Python/KNIME**, **API internal untuk aplikasi**, dan **Data Hub (DMZ) → 3scale/SFTP → Mitra**. Panah putus-putus bertanda silang dari Silver langsung ke "core system" dan ke "Mitra (DB langsung)" menggambarkan jalur yang dihentikan. Di atas: gembok "Ranger tag + izin BI per klasifikasi"; di bawah: kotak parameter "KPI · sertifikasi · freshness · akses · rasionalisasi".
