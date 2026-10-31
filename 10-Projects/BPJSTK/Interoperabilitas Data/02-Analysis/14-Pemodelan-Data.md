---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Pemodelan & Desain Data
sesi: 4
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.1, 5.3, 6]
status: draft-diskusi
acuan_internal: DAMA-DMBOK v2, bab Data Modeling & Design
sumber: [D3 model konseptual Kepesertaan & Pelayanan, R1 rekaman diskusi, D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, 00-Analisis-Dokumen-BPJS.md, 11-Master-Data.md, 12-Integrasi.md, 13-Arsitektur-Data.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 77 = cetak '- 76 -')."
---

# Sesi 4 — Pemodelan & Desain Data

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI (kebijakan, kondisi yang seharusnya). **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (paparan kondisi saat ini/as-is). "hlm." merujuk pada nomor halaman PDF.

> **Kerangka sesi.** Konsisten dengan sesi 1–3: ESDP adalah platform poros; integrasi dibagi upstream (sumber → platform) dan downstream (platform → pengguna data); arsitektur target **Opsi B** dengan zona staging | lake | DWH | serving ditambah ODS di atas Cloudera; MDM sebagai rujukan ([[13-Arsitektur-Data]]). Blueprint menetapkan **standar dan parameter yang wajib didefinisikan**, bukan nilainya. Sesi ini mencakup area penuh: model enterprise, model fisik di sistem informasi, standar penamaan, serta pola pemodelan platform (medallion dan pola DWH).

## 1. Ringkasan
- **Model data konseptual sudah dimulai, model logis/fisik dan tata kelolanya belum terbukti.** D2 memiliki Data Building Block bertingkat L0–L3 (D2 hlm. 15–18) dan Enterprise Data Model as-is per domain (D2 hlm. 29–30). Namun dokumentasi DBB masih mengacu ITMP 2019–2024, dan sebagian entitas masih diolah dengan spreadsheet (D2 hlm. 31).
- **Kebijakan D1 cukup jelas.** Ps. 14 mewajibkan EDM yang dikelola Unit Manajemen Data, prinsip formalisasi, ruang lingkup, dan dokumentasi, serta Physical Data Model (PDM) aplikasi yang direviu kesesuaiannya dengan Kamus Data dan EDM (D1 hlm. 16–17). Standar Kamus Data memuat 11 atribut termasuk nama elemen fisik/logis dan lokasi tabel (D1 hlm. 75).
- **Pola pemodelan platform tidak terdokumentasi.** D2 menunjukkan alur Staging → Data Lake → DWH/data mart (D2 hlm. 20–21), tanpa bukti pola model DWH, penanganan historis (SCD), atau dimensi yang mengacu master data. Big data tidak mengacu master data (D2 hlm. 24).
- **Skor usulan rata-rata ±1,5** (31 Okt 2026: **±1,6**, PD-5 dan PD-7 naik), terendah di antara sesi 1–4. Ini wajar: pemodelan adalah area yang paling bergantung pada praktik, bukan kebijakan.
- **Rekomendasi (usulan, untuk disepakati): pola hybrid.** Medallion dipetakan ke zona Opsi B. Lapis **silver** memakai **Data Vault 2.0** untuk domain inti (kepesertaan, iuran, klaim), lapis **gold** memakai **Kimball dimensional** dengan conformed dimension dari MDM. Inmon 3NF tidak dipilih sebagai inti karena paling rentan terhadap perubahan SMILE. Pilihan ini didasarkan pada perbandingan empat opsi (bagian 6).

## 2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Data Building Block (konseptual) | DBB L0 batasan, L1 data block, L2 data domain, L3 data entity | D2 hlm. 15 | Contoh blok: Corporate Planning, Business Services, Enterprise Support (D2 hlm. 16–18) |
| Enterprise Data Model | Relasi antar-entitas per domain (mis. Kepesertaan, Pelayanan) | D2 hlm. 29–30 | Hanya 2 dari 11 halaman tersedia di dokumen; ada legenda "entitas belum diolah secara tersistem" (D2 hlm. 29–30) |
| Kondisi & risiko EDM | Dokumentasi DBB mengacu ITMP 2019–2024; sebagian data diolah spreadsheet; banyak pengelolaan data belum terhubung antar-proses | D2 hlm. 31 | Risiko: human error, sulit mengelola volume besar, unit bisnis sulit mengetahui perubahan terbaru (D2 hlm. 31) |
| Kamus Data & Business Glossary | Standar penyusunan sama dengan D1; pemeliharaan minimal 1 kali setahun | D2 hlm. 25 | Kelengkapan isi dan cakupan [perlu konfirmasi] |
| Katalog metadata | Dataedo (Oracle) dan Apache Atlas (Cloudera), terpisah | D2 hlm. 26 | Model platform tidak terhubung ke satu katalog |
| Physical Data Model aplikasi | Tidak ada bukti PDM dan reviu kesesuaian terhadap EDM | – | [perlu konfirmasi] |
| Model platform data | Staging (ADG) → Data Lake → DWH, EDW, data mart di Cloudera | D2 hlm. 20–21 | Pola model (Kimball/Inmon/Vault), SCD, dan standar penamaan tidak disebut [perlu konfirmasi] |
| Master data dalam model | Master data manual; big data tidak mengacu master data | D2 hlm. 24 | Dimensi tidak conformed, lihat [[11-Master-Data]] |
| Konsumsi model | 17 dashboard (Tableau, QlikSense); analisis via request ke MDT | D2 hlm. 22 | Belum ada lapisan semantik/self-service |

## 3. Kondisi yang seharusnya (D1)
- **Tujuan (Ps. 14 ayat 1):** analisis dan penyampaian model data tepat dan efisien; kesamaan perspektif dan keselarasan kebutuhan saat ini dan mendatang (D1 hlm. 16).
- **Cakupan (Ps. 14 ayat 2):** EDM serta model dan desain data dalam sistem informasi (D1 hlm. 16).
- **Pengelolaan EDM (Ps. 14 ayat 3):** identifikasi kebutuhan bersama UPTI, penyusunan berdasarkan model eksisting, reviu untuk quality control dan perbaikan berkelanjutan (D1 hlm. 16).
- **Prinsip (Ps. 14 ayat 4):** formalisasi struktur dan relasi untuk meminimalkan anomali, penetapan ruang lingkup, dan dokumentasi sebagai referensi proyek berikutnya (D1 hlm. 16–17).
- **Model di sistem informasi (Ps. 14 ayat 5):** Unit Pengembangan TI menyusun PDM sesuai Kamus Data dan EDM; Unit Manajemen Data mereviu kesesuaiannya (D1 hlm. 17).
- **Standar Kamus Data:** DBB, domain, entity, physical/logical element data, format type/length, table location, business rules, deskripsi, contoh nilai valid; diperbarui minimal setahun sekali (D1 hlm. 75).
- **Komponen DWBI:** staging, master data & data referensi sebagai rujukan DWH, central warehouse yang mengelola **data atomic historis**, ODS non-historis dengan latensi rendah, data mart untuk presentasi, cubes/OLAP (D1 hlm. 77).
- **Pengembangan DWH:** pemetaan sumber ke target dan remediasi/transformasi, populasi DWH, portofolio BI, pemeliharaan produk data (D1 hlm. 79).

## 4. Tabel penilaian

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal. Setiap skor harus punya bukti.

| Aspek                                        | Praktik yang diharapkan                                                       | Kebijakan BPJS (D1)                                       | Kondisi saat ini (D2)                                                          | Gap                                                | Skor usulan (1-5)    |
| -------------------------------------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------- | -------------------- |
| PD-1. Model konseptual (DBB)                 | DBB/domain/entitas lengkap, terpelihara, dipakai proyek                       | Ps. 13–14 (D1 hlm. 15–16)                                 | DBB L0–L3 ada (D2 hlm. 15–18); mengacu ITMP 2019–2024 (D2 hlm. 31)             | Pemeliharaan tidak terbukti                        | 2                    |
| PD-2. Enterprise Data Model                  | EDM logis per domain, relasi, dikelola UMD, direviu                           | Ps. 14(3) (D1 hlm. 16)                                    | EDM as-is sebagian (D2 hlm. 29–30); entitas belum tersistem (D2 hlm. 29–31)    | Kelengkapan dan siklus reviu belum terbukti        | 2                    |
| PD-3. PDM aplikasi & reviu kesesuaian        | PDM tiap aplikasi, direviu terhadap EDM dan Kamus Data                        | Ps. 14(5) (D1 hlm. 17)                                    | Tidak ada bukti                                                                | Proses reviu belum tampak                          | 1 [perlu konfirmasi] |
| PD-4. Kamus Data & standar penamaan          | Kamus data lengkap; konvensi penamaan fisik/logis per lapisan                 | Standar Kamus Data (D1 hlm. 75)                           | Standar disalin di D2 hlm. 25; konvensi penamaan platform tidak ada            | Standar ada, penerapan dan konvensi platform belum | 2                    |
| PD-5. Pola pemodelan platform (lapisan)      | Pola per lapisan terdokumentasi (raw, terintegrasi, presentasi)               | Komponen DWBI (D1 hlm. 77)                                | Staging → Lake → DWH/mart (D2 hlm. 20–21), pola model tidak disebut            | Pola belum ditetapkan                              | 1 [pk] → **2** (31 Okt 2026): Kudu untuk CDC, Parquet untuk analitik, data mart Kudu/Parquet via Talend; lapis hanya dibedakan skema (R1 [S 00:17:55–00:19:21; S 00:29:19–00:31:48; S 00:44:54–00:45:06]) |
| PD-6. Conformed dimension & MDM              | Dimensi kunci (peserta, PK/BU, kantor, program) dari MDM, dipakai lintas mart | MD&DR rujukan DWH (D1 hlm. 77)                            | Big data tidak mengacu master data (D2 hlm. 24)                                | Dimensi tidak conformed                            | 1                    |
| PD-7. Historis, SCD & auditability           | Data atomic historis, kebijakan SCD per atribut, lineage ke sumber            | Central warehouse atomic historis (D1 hlm. 77)            | Tidak ada bukti penanganan historis/SCD                                        | Belum ditetapkan                                   | 1 [pk] → **2** (31 Okt 2026): SCD sudah diterapkan [tipe per tabel perlu konfirmasi] (R1 [S 00:00:37–00:00:49]); lineage belum ada (R1 [P 01:36:23–01:36:41]) |
| PD-8. Tata kelola & versi model              | Siklus reviu, versioning, analisis dampak perubahan sumber (SMILE)            | Reviu quality control (D1 hlm. 16)                        | Unit bisnis sulit mengetahui perubahan terbaru (D2 hlm. 31)                    | Change management model belum ada                  | 1                    |
| PD-9. Data contract / skema antarmuka        | Skema, semantik, dan parameter per interface tercatat di katalog              | Ps. 66 (D1 hlm. 50–51); lihat [[12-Integrasi]]            | Tidak ada bukti data contract                                                  | Belum ada                                          | 1                    |
| PD-10. Cakupan model terhadap data tersistem | Seluruh entitas kritikal tersistem dan termodelkan                            | Formalisasi & ruang lingkup (D1 hlm. 16)                  | Sebagian data diolah spreadsheet (D2 hlm. 31)                                  | Entitas di luar sistem                             | 2                    |
| PD-11. Model presentasi (mart, BI)           | Mart dimensional, lapisan semantik, self-service                              | Data mart, cubes (D1 hlm. 77); portofolio BI (D1 hlm. 79) | Data mart dan 17 dashboard (D2 hlm. 20–22); request ke MDT lambat (D2 hlm. 22) | Belum ada lapisan semantik dan cubes               | 2                    |

**Rata-rata: ±1,5** → **±1,6** (31 Okt 2026).

## 5. Medallion: pemetaan ke zona Opsi B

**Gambar 1.** Lapisan medallion dan pola pemodelan per lapisan (usulan).

![[14-Medallion-Pemodelan.excalidraw]]

| Lapis medallion | Zona Opsi B | Isi | Pola model (usulan) | Historis | Pengguna utama |
|---|---|---|---|---|---|
| (landing) | Staging | Salinan sementara dari CDC/batch/API | Mengikuti sumber | Tidak; retensi pendek | Proses ingest |
| **Bronze** | Lake – raw | Data apa adanya, append-only, termasuk tidak terstruktur | Skema sumber + kolom teknis (waktu muat, sistem sumber, batch) | Ya, seluruh perubahan | Data engineer, audit |
| **Silver** | DWH – core terintegrasi (dan lake curated untuk data science) | Data dibersihkan, kunci bisnis diselaraskan ke MDM, terintegrasi lintas sumber | **Data Vault 2.0** (hub, link, satellite) untuk domain inti; tabel curated untuk domain sederhana | Ya, atomic historis (D1 hlm. 77) | Data engineer, data scientist |
| **Gold** | Serving – data mart, cubes | Produk data siap pakai per kebutuhan bisnis | **Kimball** star schema, conformed dimension dari MDM, SCD sesuai kontrak | Sesuai kebutuhan mart | BI, aplikasi, mitra, DaaS |
| (operasional) | ODS | Data terkini latensi rendah | Model relasional ringkas per kebutuhan layanan | Tidak (D1 hlm. 77) | Layanan downstream operasional |

Catatan:
- ODS **bukan** bagian alur medallion. ODS diisi langsung dari CDC dan melayani kebutuhan operasional; gold tetap menjadi sumber analitik.
- Format tabel di Cloudera (mis. Hive/Impala, Iceberg) [perlu konfirmasi]; pilihan format memengaruhi biaya SCD dan time-travel.

## 6. Opsi pola model DWH

| Opsi | Deskripsi |
|---|---|
| A. Kimball murni | Seluruh DWH sebagai star schema dengan bus matrix dan conformed dimension |
| B. Inmon 3NF | EDW ternormalisasi (3NF) sebagai inti, data mart dimensional di atasnya |
| C. Data Vault 2.0 murni | Raw vault + business vault sebagai inti; konsumen membaca view/mart di atas vault |
| D. Hybrid (Vault di silver + Kimball di gold) | Data Vault 2.0 untuk domain inti di silver; star schema conformed di gold; domain sederhana boleh curated langsung |

**Perbandingan (skala: ++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria (konteks BPJS)                                            | A. Kimball | B. Inmon 3NF | C. Data Vault | D. Hybrid |
| ------------------------------------------------------------------ | ---------- | ------------ | ------------- | --------- |
| Auditability & data atomic historis (D1 hlm. 77)                   | −          | +            | ++            | ++        |
| Toleransi perubahan sumber (SMILE monolitik, D2 hlm. 37)           | −          | −−           | ++            | ++        |
| Integrasi banyak sumber & 5 jenis DB (D2 hlm. 37–38)               | 0          | +            | ++            | ++        |
| Keselarasan dengan MDM & conformed dimension (D2 hlm. 24)          | ++         | +            | +             | ++        |
| Waktu ke manfaat untuk 17 dashboard (D2 hlm. 22)                   | ++         | −            | −             | +         |
| Kemudahan konsumsi / self-service (D1 hlm. 76)                     | ++         | −            | −−            | ++        |
| Kompleksitas & kebutuhan SDM/otomasi                               | +          | 0            | −             | −         |
| Kecocokan dengan zona Opsi B dan desain inkremental (D1 hlm. 76)   | 0          | −            | +             | ++        |
| Kinerja query analitik di Cloudera [perlu konfirmasi format tabel] | ++         | 0            | −             | +         |

**Rekomendasi (usulan): Opsi D, hybrid.**
- **Silver memakai Data Vault 2.0** untuk domain inti (kepesertaan, iuran, klaim, dan lainnya sesuai prioritas). Alasannya: sumber banyak dan heterogen, kebutuhan historis dan audit sesuai D1 hlm. 77, serta perubahan SMILE cukup ditambahkan sebagai satellite baru tanpa merombak model.
- **Gold memakai Kimball** agar BI, aplikasi, dan mitra mendapat model yang mudah dipakai. Conformed dimension diambil dari MDM.
- **Domain sederhana atau referensi** boleh langsung curated di silver tanpa vault, untuk menghindari kompleksitas yang tidak perlu.
- **Prasyarat:** otomasi pembangkitan vault (template atau tool) dan peningkatan keterampilan tim. Tanpa itu, Opsi A (Kimball murni) menjadi fallback yang lebih realistis [perlu konfirmasi kapasitas tim].
- **Opsi B (Inmon 3NF) tidak dipilih** karena biaya perubahan model tertinggi saat sumber berubah. **Opsi C (Vault murni)** menyulitkan konsumen tanpa lapisan gold.

## 7. Standar pemodelan (usulan Blueprint)
Blueprint menetapkan standar dan parameter yang wajib ada; nilainya diisi per domain.

1. **Konvensi penamaan.** Wajib menetapkan: prefiks lapis (mis. `brz_`, `slv_`, `gld_`), pola `domain_entitas`, jenis objek (hub/link/sat/dim/fact), bahasa istilah (Indonesia/Inggris), dan huruf (snake_case). Nama logis mengikuti Business Glossary; nama fisik dicatat di Kamus Data (D1 hlm. 75).
2. **Kunci.** Business key mengacu ID MDM (mis. NIK, NPP) [perlu konfirmasi kunci resmi]; surrogate/hash key ditetapkan per lapis.
3. **Conformed dimension dari MDM.** Kandidat: peserta/tenaga kerja, pemberi kerja/badan usaha, kantor (cabang/wilayah), program (JHT, JKK, JKM, JP, JKP), waktu, kanal (berdasarkan domain D2 hlm. 16). Pemilik setiap dimensi ditetapkan.
4. **Historis dan SCD.** Kebijakan SCD ditetapkan per atribut (tipe 1 atau 2 sebagai default); silver selalu menyimpan historis penuh.
5. **Kolom teknis wajib:** waktu muat, sistem sumber, ID batch/run, hash record, dan penanda hapus.
6. **Data contract per dataset/interface** (terhubung ke [[12-Integrasi]]): skema, semantik, pemilik, klasifikasi (D1 hlm. 93–94), freshness, aturan kualitas, kebijakan perubahan skema, dan retensi.
7. **Siklus hidup model:** EDM → model logis → PDM; reviu kesesuaian oleh UMD (D1 hlm. 17); versioning; analisis dampak sebelum perubahan skema sumber.
8. **Dokumentasi di satu katalog:** model, lineage bronze → silver → gold terdaftar di katalog terpadu (D2 hlm. 26).

## 8. Batas peran

| Peran / komponen | Tanggung jawab model | Bukan tanggung jawab |
|---|---|---|
| Unit Manajemen Data | Pemilik EDM, standar penamaan, reviu PDM, model silver/gold (D1 hlm. 16–17) | Bukan penyusun PDM aplikasi |
| Unit Pengembangan TI / pemilik SoR | Menyusun PDM aplikasi sesuai Kamus Data & EDM (D1 hlm. 17); memberi tahu perubahan skema | Bukan pemilik model analitik |
| MDM | Sumber kunci dan atribut conformed dimension | Bukan model analitik atau fakta |
| ESDP / Cloudera | Menjalankan lapis bronze/silver/gold dan ODS | Bukan pemilik definisi bisnis |
| Unit Bisnis (data owner) | Definisi bisnis (glossary), kebutuhan mart, persetujuan data contract | Bukan perancang model fisik |

## 9. Keterkaitan dengan Blueprint
- **3.1–3.3 (as-is & gap):** bagian 2–4.
- **3.4 (prinsip/standar):** bagian 7.
- **4 (arsitektur target):** bagian 5–6, melengkapi Opsi B di [[13-Arsitektur-Data]].
- **5.3 (standar data):** konvensi penamaan, SCD, dan data contract.
- **6 (roadmap):** tema besar per tahun (usulan, untuk disepakati):
  - **Tahun 1 — Fondasi:** standar penamaan & model, pembaruan EDM, bronze untuk seluruh sumber prioritas, silver untuk 1–2 domain pilot (mis. kepesertaan & iuran) [usulan].
  - **Tahun 2 — Layanan:** gold dengan conformed dimension dari MDM; migrasi dashboard ke mart gold; data contract untuk interface downstream.
  - **Tahun 3 — Optimasi:** otomasi pembangkitan model, lapisan semantik untuk self-service, perluasan ke domain lain.

## 10. Pertanyaan terbuka untuk BPJS
1. Apakah EDM lengkap (11 halaman) dan model logis per domain tersedia, dan kapan terakhir diperbarui?
2. Apakah PDM aplikasi (terutama SMILE) tersedia dan pernah direviu terhadap EDM sesuai Ps. 14(5)?
3. Pola model apa yang dipakai di DWH/EDW Cloudera saat ini (star, 3NF, lain)?
4. Bagaimana historis dan perubahan atribut ditangani saat ini?
5. Apakah ada konvensi penamaan untuk database, tabel, dan kolom di platform?
6. Format tabel dan engine apa yang dipakai di Cloudera (Hive, Impala, Iceberg, Kudu)?
7. Kunci identitas resmi apa yang dipakai untuk peserta dan pemberi kerja (NIK, KPJ, NPP)?
8. Bagaimana perubahan skema SMILE dikomunikasikan ke tim data?
9. Berapa kapasitas tim data engineer dan modeler, dan apakah ada tool pemodelan atau otomasi?

## 11. Poin diskusi (ikpu)
- [ ] Sepakati pemetaan medallion ke zona Opsi B (staging = landing, lake = bronze, DWH = silver, serving = gold; ODS terpisah).
- [ ] Sepakati Opsi D hybrid (Vault di silver, Kimball di gold) sebagai rekomendasi, dengan Opsi A sebagai fallback jika kapasitas/otomasi tidak memadai.
- [ ] Tetapkan daftar conformed dimension prioritas dan pemiliknya bersama sesi Master Data.
- [ ] Sepakati elemen wajib standar penamaan dan data contract (tanpa nilai) untuk Blueprint 5.3.
- [ ] Validasi skor, terutama PD-3, PD-5, dan PD-7 yang bertanda [perlu konfirmasi].

## Keputusan Diskusi (26 Okt 2026)
1. **Pemetaan medallion ke zona Opsi B (ODS di luar alur):** diterima sementara, akan direvisit.
2. **Rekomendasi Hybrid (Data Vault di silver, Kimball di gold; Kimball murni sebagai fallback):** akan direvisit.
3. **Shared (conformed) dimension:** daftar sementara dan urutan prioritas: Peserta, Pemberi Kerja, Kantor, Program, Kanal. Blueprint memuat kriteria penetapannya (di bawah).
4. **Standar penamaan & data contract:** Blueprint cukup memuat elemen wajib, tanpa nilai.

### Kriteria Penetapan Shared Dimension
Sebuah entitas dijadikan shared dimension bila memenuhi kriteria wajib dan minimal satu kriteria penguat.

**Wajib**
- **Lintas proses/domain:** dipakai oleh ≥ 2 proses bisnis atau data mart (mis. kepesertaan, iuran, klaim).
- **Identitas stabil:** punya kunci bisnis yang jelas dan stabil (mis. NIK, NPP, kode kantor), atau dapat diberi kunci dari MDM.
- **Ada pemilik:** ada data owner/steward yang disepakati untuk definisi dan kualitasnya.

**Penguat**
- Merupakan entitas master/reference data di MDM (sesi 1).
- Dipakai untuk pelaporan lintas unit atau ke pihak eksternal (regulator, K/L).
- Menjadi sumber inkonsistensi angka antarlaporan saat ini.
- Atributnya perlu riwayat yang seragam (SCD) di semua mart.

**Konsekuensi penetapan**
- Satu definisi, satu kunci, satu sumber (MDM/silver) untuk semua mart di gold.
- Perubahan struktur melalui tata kelola model (versi dan persetujuan owner).
- Tercatat di katalog metadata beserta owner dan data contract-nya.


## Pembaruan 2026-10-31

**Bukti baru (kondisi saat ini):**
- **Model konseptual Kepesertaan** (D3-04, "Gambar 17") dan **Pelayanan** (D3-06, "Gambar 27") serta daftar 13 entitas Kepesertaan (D3-05). Model Kepesertaan memuat antara lain Individu, Peserta, Perusahaan, GrupPerusahaan, KantorPerusahaan, PemberiKerja, UnitPemberiKerja, Kantor, Pendaftaran, HubunganKerja, LaporanUpah, IuranProgramPekerja, SaldoProgramPeserta, SumberData. Model Pelayanan berpusat pada KasusKlaim dengan subtipe per program (UsiaTua, Pensiun, Kematian, KecelakaanKerja, KehilanganPekerjaan, PenempatanPMI).
- **Platform:** CDC ke Kudu, Parquet untuk analitik, data mart Kudu/Parquet via Talend; SCD sudah diterapkan (R1 [S 00:00:37–00:00:49; S 00:17:55–00:19:21; S 00:29:19–00:31:48]).
- BPJS bertanya apakah kepesertaan termasuk dimensi atau fakta (R1 [P 01:32:39–01:33:45]). Usulan jawaban: **Peserta** = dimensi (shared); **kepesertaan/pendaftaran per pemberi kerja dan periode** = fakta (periodic snapshot / accumulating) atau tabel link di silver.
- Model peserta lama satu baris per peserta–program dengan pengulangan data individu (R1 [P 01:21:33–01:22:26]); model konseptual sudah memisahkan Individu dan Peserta.

### Pemeriksaan shared dimension terhadap entitas model konseptual (D3-04, D3-05, D3-06)

| Shared dimension (Keputusan 3) | Entitas model konseptual | Kesesuaian | Catatan desain |
|---|---|---|---|
| **Peserta** | **Individu** (orang; NIK) dan **Peserta** (individu yang terdaftar; kode TK/KPJ), plus AlamatIndividu, Keluarga, AnggotaKeluarga | Sebagian: model memisahkan orang dan peran, sedangkan nama dimensi "Peserta" mencampur keduanya | Opsi di bawah |
| **Pemberi Kerja** | **Perusahaan** (badan; potensi atau terdaftar), **GrupPerusahaan**, **PemberiKerja** (peran pemberi kerja; NPP), **UnitPemberiKerja** (unit pengelola administrasi), KantorPerusahaan, ProyekJakon, PelaksanaPenempatanPMI, PemilikPerusahaan | Sebagian: empat tingkat (grup → perusahaan → pemberi kerja → unit) | Dimensi Pemberi Kerja pada grain **UnitPemberiKerja** dengan hierarki ke PemberiKerja, Perusahaan, GrupPerusahaan; ProyekJakon dan P3MI sebagai subtipe PemberiKerja |
| **Kantor** | **Kantor** (hierarki "bagian dari") | Sesuai | Dimensi berhierarki; SCD tipe 2 untuk perubahan struktur kantor |
| **Program** | Tidak ada entitas master; program tersirat di IuranProgramPekerja, SaldoProgramPeserta, PiutangProgram, dan subtipe KasusKlaim | Belum ada entitas | Tambahkan entitas reference data **Program** (JHT, JKK, JKM, JP, JKP) ke model konseptual |
| **Kanal** | Tidak ada; terdekat **SumberData** dan Agen/Pembina | Belum ada entitas | Tambahkan reference data **Kanal** (mis. JMO, kantor cabang, Lapak Asik, mitra) [perlu konfirmasi daftar kanal] |

**Perusahaan vs Pemberi Kerja vs Unit Pemberi Kerja.** Perusahaan adalah badan usaha (dapat berupa potensi, belum terdaftar). Pemberi Kerja adalah peran terdaftar yang diberi NPP. Unit Pemberi Kerja adalah unit yang bertanggung jawab atas administrasi kepesertaan atas nama pemberi kerja (D3-05 no. 4, 8, 10, 13). Satu perusahaan dapat memiliki beberapa NPP/unit [perlu konfirmasi], dan merger mengubah relasinya (R1 [P 01:06:06–01:08:42]).

| Opsi grain dimensi Pemberi Kerja | Kelebihan | Kekurangan |
|---|---|---|
| A. Grain Perusahaan | Agregasi per badan usaha mudah | Kehilangan rincian NPP/unit; laporan iuran per NPP sulit |
| B. Grain UnitPemberiKerja + hierarki (PemberiKerja → Perusahaan → Grup) | Mendukung laporan per unit, NPP, dan grup; sesuai model konseptual | Dimensi lebih lebar; perlu SCD tipe 2 untuk perpindahan unit/merger |
| C. Tiga dimensi terpisah | Fleksibel | Banyak join; risiko inkonsistensi |

**Rekomendasi (usulan): Opsi B.**

**Individu vs Peserta.**

| Opsi | Kelebihan | Kekurangan |
|---|---|---|
| A. Satu dimensi Peserta (grain kode TK) dengan atribut individu | Sederhana untuk mart kepesertaan | Data individu berulang (masalah model lama, R1 [P 01:21:33–01:22:26]); satu orang dengan beberapa kepesertaan sulit dihitung |
| B. Dimensi **Individu** (grain NIK/golden ID) + dimensi **Peserta** (grain kode TK) yang merujuk Individu | Selaras model konseptual dan golden record ([[11-Master-Data]] §4A); hitung orang unik dan kepesertaan terpisah | Dua dimensi; perlu bridge untuk keluarga/ahli waris |
| C. Individu sebagai dimensi; kepesertaan sebagai fakta saja | Paling ramping | Atribut kepesertaan (segmen, status) tersebar di fakta |

**Rekomendasi (usulan): Opsi B.** Ganti nama butir pertama daftar sementara menjadi "Individu & Peserta" saat Keputusan 3 direvisit; urutan prioritas lain tetap.

**Celah cakupan model:** model konseptual JKP belum ada (dikonfirmasi R1); D3-04 dan D3-06 tidak menampilkan atribut, kunci, maupun kardinalitas lengkap.

| Aspek | Skor lama → baru | Alasan / sumber |
|---|---|---|
| PD-1. Model konseptual | 2 → 2 | D3-04/05/06 ada; JKP belum; Program/Kanal belum ada sebagai entitas |
| PD-5. Pola pemodelan platform | 1 [pk] → 2 | Pola Kudu/Parquet/data mart terkonfirmasi (R1); belum medallion |
| PD-6. Conformed dimension | 1 → 1 | Belum ada shared dimension yang dipraktikkan; MDM masih POC |
| PD-7. Historis & SCD | 1 [pk] → 2 | SCD diterapkan (R1); lineage belum ada |
| PD-10. Cakupan model | 2 → 2 | JKP belum dimodelkan |

**Rata-rata baru: ±1,6** (dari ±1,5).
