---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Metadata & Kualitas Data
sesi: 5
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.3, 5.4, 6]
status: draft-diskusi
acuan_internal: DAMA-DMBOK v2, bab Metadata Management & Data Quality
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, 00-Analisis-Dokumen-BPJS.md, bpjstk_tender_inventory.csv, 11-Master-Data.md, 12-Integrasi.md, 13-Arsitektur-Data.md, 14-Pemodelan-Data.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 77 = cetak '- 76 -')."
---

# Sesi 5 — Metadata & Kualitas Data

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI (kebijakan, kondisi yang seharusnya). **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (paparan kondisi saat ini/as-is). "hlm." merujuk pada nomor halaman PDF.

> **Kerangka sesi.** Konsisten dengan sesi 1–4: ESDP adalah platform poros; integrasi dibagi upstream dan downstream; zona Opsi B (staging | lake | DWH | serving + ODS) dipetakan ke medallion (landing, bronze, silver, gold); pola pemodelan Hybrid masih akan direvisit ([[14-Pemodelan-Data]]); shared dimension sementara: Peserta, Pemberi Kerja, Kantor, Program, Kanal. Blueprint menetapkan **elemen dan parameter wajib**, bukan nilainya. Dokumen ini terdiri atas dua bagian: **Bagian A — Metadata** dan **Bagian B — Kualitas Data**.

## 1. Ringkasan
- **Kebijakan keduanya sudah rinci; praktiknya baru sebagian dan terfragmentasi.** D1 mengatur prinsip metadata, glossary, dan Kamus Data 11 atribut (D1 hlm. 74–75), serta siklus kualitas data lengkap: metrik, profiling, program, monitoring bulanan, evaluasi triwulan, dimensi, formula, dan skala (D1 hlm. 80–85).
- **Metadata: dua katalog dan definisi yang tidak konsisten.** Dataedo dipakai untuk Oracle dan Apache Atlas untuk Cloudera (D2 hlm. 26). Glossary dan Kamus Data memuat istilah dan elemen dengan nama/definisi berbeda; kewenangan perubahan elemen data tidak jelas; struktur data baru sulit terdeteksi (D2 hlm. 26).
- **Kualitas data: prosedur ada, hasil belum memadai.** Prosedur metrik → profiling → penilaian → cleansing → tindak lanjut → monitoring ada (D2 hlm. 27). Namun masih ada duplikasi, inkonsistensi, dan format tidak valid; tool otomatis belum memberi rekomendasi perbaikan; perbaikan bad data di kantor cabang belum optimal; awareness entry data rendah (D2 hlm. 28).
- **Celah utama untuk ESDP:** belum ada lineage end-to-end, metadata operasional (freshness, volume, status run), quality gate per lapis, maupun data contract.
- **Skor usulan:** Metadata **±1,4**, Kualitas Data **±1,6**.
- **Rekomendasi (usulan, berbasis perbandingan):**
  - **Metadata:** federasi katalog. Atlas untuk metadata teknis dan lineage di Cloudera; Dataedo sebagai katalog bisnis (glossary, Kamus Data, owner) dan titik masuk pengguna. Katalog enterprise tunggal dievaluasi di Tahun 3.
  - **Kualitas data:** aturan didefinisikan sekali (di katalog/data contract), dijalankan di dua titik: validasi di sumber saat entri dan quality gate per lapis medallion. Talend dipakai ulang sebagai mesin profiling dan aturan.

---

# Bagian A — Metadata

## A2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Katalog teknis Oracle | Dataedo | D2 hlm. 26 | Lisensi Dataedo tercatat di tender publik 2024 (00-Analisis) |
| Katalog & lineage Cloudera | Apache Atlas: capture metadata dan lineage data di Cloudera | D2 hlm. 26 | Tidak ada di tender; kemungkinan bawaan platform Cloudera [perlu konfirmasi] |
| Business Glossary | Standar 4 atribut (terminologi, sumber definisi, definisi, keterangan) | D1 hlm. 74; D2 hlm. 25 | Ada istilah dengan definisi berbeda di lebih dari satu bidang (D2 hlm. 26) |
| Kamus Data | Standar 11 atribut, pembaruan minimal setahun sekali | D1 hlm. 75; D2 hlm. 25 | Nama dan definisi elemen berbeda antar-skema (D2 hlm. 26) |
| Deteksi perubahan struktur | Tool belum memberi insight jika ada struktur data baru | D2 hlm. 26 | Risiko data baru tidak teridentifikasi |
| Kewenangan metadata | Kewenangan pengubahan elemen data tidak ditentukan jelas | D2 hlm. 26 | Risiko perubahan tanpa akuntabilitas |
| Metadata operasional | Tidak ada bukti (status run, volume, freshness) | – | [perlu konfirmasi] |
| Data contract | Tidak ada bukti | – | Lihat [[12-Integrasi]], [[14-Pemodelan-Data]] |

**Tiga jenis metadata yang dibutuhkan ESDP:**
- **Bisnis:** glossary, definisi, owner/steward, klasifikasi data (D1 hlm. 94), CDE.
- **Teknis:** skema, tipe, lokasi tabel, lapis medallion, lineage sumber → ingest → bronze → silver → gold → BI.
- **Operasional:** jadwal dan status run, volume, freshness aktual, hasil quality gate, insiden.

## A3. Kondisi yang seharusnya (D1)
- **Ps. 15:** Unit Manajemen Data mengelola metadata sesuai proses bisnis; terdiri atas glossary dan Kamus Data; dievaluasi minimal setahun sekali (D1 hlm. 17).
- **Katalog Data:** sistem terpusat yang mendokumentasikan metadata untuk menemukan, memahami, dan menggunakan data (D1 hlm. 5).
- **Prinsip metadata:** komitmen organisasi, strategi, perspektif enterprise, sosialisasi, akses, kualitas, perbaikan berkelanjutan (D1 hlm. 74).
- **Business Glossary:** 4 atribut standar (D1 hlm. 74).
- **Kamus Data:** 11 atribut, yaitu DBB, data domain, data entity, physical element, logical element, format type, format length, table location, business rules, deskripsi, contoh nilai valid (D1 hlm. 75).
- **DWBI:** prinsip integrasi dengan metadata (D1 hlm. 76).

## A4. Tabel penilaian — Metadata

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal.

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| MK-1. Business Glossary | Satu definisi per istilah, owner jelas, dipakai di laporan | D1 hlm. 17, 74 | Ada; definisi berbeda antarbidang (D2 hlm. 25–26) | Konsistensi dan owner | 2 |
| MK-2. Kamus Data | 11 atribut terisi untuk CDE dan domain prioritas | D1 hlm. 75 | Ada; nama/definisi berbeda antar-skema (D2 hlm. 26) | Harmonisasi | 2 |
| MK-3. Katalog terpusat | Satu titik temu metadata lintas platform | D1 hlm. 5 | Dataedo dan Atlas terpisah (D2 hlm. 26) | Federasi/integrasi | 2 |
| MK-4. Lineage end-to-end | Sumber → ingest → lapis → BI | Integrasi metadata DWBI (D1 hlm. 76) | Atlas hanya di Cloudera (D2 hlm. 26) | Lineage lintas platform belum ada | 1 |
| MK-5. Metadata operasional | Status run, volume, freshness tercatat otomatis | – (tidak eksplisit) | Tidak ada bukti | Belum ada | 1 [perlu konfirmasi] |
| MK-6. Kewenangan & stewardship metadata | Owner/steward per elemen, alur persetujuan | Peran UMD (D1 hlm. 17) | Kewenangan tidak jelas (D2 hlm. 26) | RACI belum ada | 1 |
| MK-7. Deteksi perubahan skema | Perubahan struktur terdeteksi dan dinilai dampaknya | Prinsip quality & improvement (D1 hlm. 74) | Tool tidak memberi insight struktur baru (D2 hlm. 26) | Belum ada | 1 |
| MK-8. Data contract | Kontrak per dataset/interface tersimpan di katalog | Tidak eksplisit; Ps. 66 hanya mengatur teknologi pertukaran (D1 hlm. 51) | Tidak ada bukti | Belum ada | 1 |

**Rata-rata Metadata: ±1,4.**

## A5. Opsi katalog metadata

| Opsi | Deskripsi |
|---|---|
| A. Status quo + proses | Dataedo dan Atlas tetap terpisah; perbaikan proses glossary/kamus |
| B. Federasi Dataedo + Atlas | Atlas = metadata teknis & lineage Cloudera; Dataedo = katalog bisnis (glossary, Kamus Data, owner, CDE) sekaligus portal pengguna; metadata Atlas disinkronkan ke Dataedo |
| C. Katalog enterprise tunggal baru | Satu katalog (komersial atau open source) menggantikan keduanya |
| D. Atlas sebagai katalog tunggal | Seluruh metadata, termasuk Oracle dan glossary, dipusatkan di Atlas |

**Perbandingan (++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria | A | B | C | D |
|---|---|---|---|---|
| Memenuhi definisi katalog terpusat (D1 hlm. 5) | −− | + | ++ | + |
| Lineage end-to-end lintas Oracle–Cloudera–BI | −− | + [perlu konfirmasi konektor] | ++ | 0 |
| Reuse investasi (Dataedo, Atlas) | ++ | ++ | −− | + |
| Kemudahan bagi pengguna bisnis (glossary, pencarian) | 0 | + | ++ | − |
| Waktu ke manfaat | ++ | + | −− | 0 |
| Biaya & risiko pengadaan | ++ | + | −− | + |
| Dukungan data contract & metadata operasional | − | 0 | ++ | 0 |

**Rekomendasi (usulan): Opsi B, dengan Opsi C dievaluasi di Tahun 3.** Opsi B memenuhi definisi katalog terpusat secara logis tanpa pengadaan baru, dan bisa berjalan di Tahun 1. Kemampuan sinkronisasi Dataedo–Atlas dan konektor lineage ke Talend, Qlik Replicate, dan Tableau/QlikSense harus diverifikasi lebih dulu [perlu konfirmasi]. Jika konektor tidak memadai, Opsi C menjadi kandidat utama.

## A6. Elemen wajib metadata (usulan Blueprint)
Per dataset/produk data, katalog wajib memuat (nilai diisi per domain):
1. Identitas: nama, domain/DBB, lapis medallion, zona Opsi B.
2. Bisnis: definisi (tautan glossary), owner, steward, klasifikasi data (D1 hlm. 94), CDE.
3. Teknis: skema dan 11 atribut Kamus Data (D1 hlm. 75), lineage hulu dan hilir.
4. Operasional: jadwal, status run terakhir, volume, freshness aktual vs target, skor kualitas.
5. Kontrak: tautan data contract (skema, semantik, freshness, aturan kualitas, kebijakan perubahan, retensi).
6. Tata kelola: versi, riwayat perubahan, persetujuan.

---

# Bagian B — Kualitas Data

## B2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Prosedur DQ | Penetapan metrik → profiling → penilaian → cleansing → tindak lanjut → evaluasi & monitoring | D2 hlm. 27 | Selaras dengan siklus D1 hlm. 80–83 |
| Temuan kualitas | Duplikasi, inkonsistensi, format tidak valid pada data unit bisnis | D2 hlm. 28 | Risiko error dalam pengelolaan dan analitik |
| Tool DQ | Ada automated DQ tool, belum ada fitur rekomendasi perbaikan | D2 hlm. 28 | Tool tidak dinamai; dugaan Talend Data Quality berdasarkan tender [perlu konfirmasi] |
| Perbaikan di cabang | Perbaikan bad data di kantor cabang belum optimal | D2 hlm. 28 | Bad data terus bertambah |
| Awareness | Awareness petugas entry belum menyeluruh | D2 hlm. 28 | Akar masalah ada di sumber |
| Pelaporan | Ada Dashboard Kualitas Data (salah satu dari 17 dashboard) | D2 hlm. 22 | Isi dan frekuensi [perlu konfirmasi] |
| Quality gate di pipeline | Tidak ada bukti | – | [perlu konfirmasi] |
| Freshness / SLA data | Tidak ada bukti target atau pemantauan | – | Lihat [[12-Integrasi]] |

## B3. Kondisi yang seharusnya (D1)
- **Metrik:** kuantitatif, formula jelas, lingkup jelas; tahapan: dimensi → CDE → formula → skala; ditetapkan tahunan dan dapat menjadi indikator kinerja unit (D1 hlm. 81).
- **Penilaian:** profiling oleh UMD; hasil diinformasikan ke Unit Bisnis, kantor wilayah, dan cabang; analisis akar masalah dan konfirmasi prioritas dengan pengguna data (D1 hlm. 81–82).
- **Program peningkatan:** strategi (hasil penilaian, dampak, rekomendasi cleansing/koreksi/perbaikan sistem/perbaikan input) dan pemetaan tanggung jawab (D1 hlm. 82).
- **Implementasi & monitoring:** UMD memimpin bersama UPTI dan Unit Bisnis; laporan bulanan ke kantor wilayah; evaluasi triwulan; task force bila perlu (D1 hlm. 83).
- **Dimensi:** accuracy, completeness, timeliness, validity, uniqueness, consistency (D1 hlm. 83–84).
- **Formula:** X = A/B × 100% untuk accuracy, completeness, timeliness, validity; X = (1 − A/B) × 100% untuk uniqueness dan consistency (contoh: NIK 16 digit; konsistensi kanal terhadap SMILE) (D1 hlm. 84).
- **Skala:** dimensi dipilih sesuai kebutuhan bisnis; skor indeks CDE berbobot: 0–60 Low, 61–90 Medium, 91–100 High; penanganan khusus untuk peserta non-aktif yang tidak terkonfirmasi (D1 hlm. 85).
- **Perubahan sistem** akibat perbaikan DQ mengikuti manajemen perubahan; teknis diatur SOP UMD (D1 hlm. 85–86).

## B4. Tabel penilaian — Kualitas Data

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| KD-1. Dimensi & metrik | Metrik per domain ditetapkan tahunan | D1 hlm. 81, 83–84 | Penetapan metrik ada di prosedur (D2 hlm. 27) | Isi metrik per domain belum terlihat | 2 |
| KD-2. CDE & skala indeks | CDE ditetapkan, bobot dan skala dipakai | D1 hlm. 85 | Tidak ada bukti daftar CDE | Belum terbukti | 1 [perlu konfirmasi] |
| KD-3. Profiling & penilaian | Profiling berkala, hasil ke unit & cabang | D1 hlm. 81–82 | Profiling dan laporan ada di prosedur (D2 hlm. 27) | Cakupan dan frekuensi | 2 |
| KD-4. Tool DQ | Otomasi aturan, profiling, rekomendasi perbaikan | – (tidak menyebut produk) | Automated tool tanpa rekomendasi perbaikan (D2 hlm. 28) | Fitur remediasi | 2 |
| KD-5. Manajemen isu & remediasi | Isu tercatat, akar masalah, SLA penanganan, perbaikan di sumber | D1 hlm. 82–83 | Perbaikan bad data di cabang belum optimal (D2 hlm. 28) | Alur isu dan SLA | 2 |
| KD-6. Quality gate per lapis | Aturan dijalankan di ingest, bronze, silver, gold | Remediasi/transformasi saat populasi DWH (D1 hlm. 79) | Tidak ada bukti | Belum ada | 1 [perlu konfirmasi] |
| KD-7. Pelaporan & monitoring | Laporan bulanan, evaluasi triwulan, dashboard | D1 hlm. 83 | Dashboard Kualitas Data (D2 hlm. 22) | Konsistensi pelaporan [perlu konfirmasi] | 2 |
| KD-8. Freshness & SLA data | Target freshness per dataset, dipantau otomatis | Timeliness (D1 hlm. 83–84) | Tidak ada bukti | Belum ada | 1 |
| KD-9. Kualitas di sumber & awareness | Validasi saat entri, pelatihan petugas | Perbaikan input (D1 hlm. 82) | Awareness entry rendah (D2 hlm. 28) | Pencegahan di sumber | 1 |

**Rata-rata Kualitas Data: ±1,6.**

## B5. Opsi pendekatan kualitas data

| Opsi | Deskripsi |
|---|---|
| A. Terpusat pasca-muat | Profiling dan cleansing berkala oleh UMD dengan tool DQ (pola saat ini) |
| B. Tertanam di pipeline | Aturan sebagai kode di setiap job; gagal → data ditahan |
| C. Di sumber saja | Validasi entri di SMILE dan aplikasi; platform tidak memeriksa |
| D. Hybrid: definisi tunggal, eksekusi berlapis | Aturan didefinisikan sekali di katalog/data contract; dijalankan sebagai validasi di sumber, quality gate per lapis medallion, dan profiling berkala; isu masuk satu alur manajemen isu |

| Kriteria | A | B | C | D |
|---|---|---|---|---|
| Mencegah bad data menyebar ke gold/mitra | − | ++ | + | ++ |
| Menangani akar masalah di sumber (D2 hlm. 28) | − | − | ++ | ++ |
| Sesuai siklus D1 (profiling, laporan bulanan, triwulan) | ++ | 0 | − | ++ |
| Reuse tool eksisting (Talend) | ++ | + | 0 | + |
| Konsistensi aturan lintas tool | 0 | − (tersebar di kode) | − | ++ |
| Kompleksitas implementasi | + | 0 | − (perubahan aplikasi) | − |

**Rekomendasi (usulan): Opsi D.** Aturan, CDE, dan ambang ditetapkan sekali dan tersimpan di katalog. Eksekusi:
- **Sumber:** validasi format dan kelengkapan saat entri untuk CDE prioritas (perubahan aplikasi melalui manajemen perubahan, D1 hlm. 85–86).
- **Pipeline:** quality gate per lapis (tabel B6), dengan Talend sebagai mesin profiling/aturan selama memadai [perlu konfirmasi kapabilitas lisensi].
- **Profiling berkala & laporan:** tetap mengikuti siklus D1.

## B6. Kualitas data per lapis medallion

| Lapis / zona | Fokus pemeriksaan | Dimensi D1 | Tindakan jika gagal |
|---|---|---|---|
| Sumber (SoR) | Validasi entri CDE | Validity, completeness | Tolak/peringatkan saat entri |
| Staging / ingest | Kelengkapan muat, skema sesuai kontrak, volume wajar | Completeness, timeliness | Ingest ditahan, notifikasi |
| Bronze | Schema drift, freshness aktual, duplikasi teknis | Timeliness, validity | Tandai, catat isu, lanjut sebagai raw |
| Silver | Aturan bisnis, dedup terhadap MDM, konsistensi dengan SMILE | Validity, uniqueness, consistency, accuracy | Karantina record, isu ke steward |
| Gold | Rekonsiliasi total ke silver/sumber, skor CDE | Consistency, accuracy | Publikasi ditahan atau ditandai |
| ODS | Latensi dan freshness | Timeliness | Alarm operasional |

## B7. Manajemen isu kualitas data
Alur: deteksi (gate/profiling/laporan pengguna) → registrasi isu (domain, CDE, dampak, klasifikasi) → triase oleh steward → analisis akar masalah (D1 hlm. 82) → perbaikan di sumber (cabang/aplikasi) dan/atau koreksi di platform → verifikasi → penutupan → pelaporan bulanan (D1 hlm. 83).

**Parameter wajib (tanpa nilai):** prioritas isu, SLA triase dan penyelesaian per prioritas, eskalasi, ambang per dimensi per CDE, target freshness per dataset, frekuensi profiling.

---

## 7. Batas peran

| Peran | Metadata | Kualitas data |
|---|---|---|
| Unit Manajemen Data | Pemilik katalog, standar glossary & Kamus Data, evaluasi tahunan (D1 hlm. 17) | Metrik, profiling, laporan bulanan, memimpin program (D1 hlm. 81–83) |
| Unit Bisnis (data owner) | Definisi istilah, persetujuan perubahan elemen | Penetapan CDE & ambang, konfirmasi prioritas (D1 hlm. 82) |
| Data steward (usulan, per domain) | Pemeliharaan definisi & kontrak | Triase dan tindak lanjut isu |
| Kantor wilayah / cabang | – | Perbaikan bad data di sumber (D1 hlm. 81, 83; D2 hlm. 28) |
| Unit Pengembangan TI / pemilik SoR | Memberi tahu perubahan skema | Validasi entri dan perbaikan sistem (D1 hlm. 85) |
| Unit Operasional TI | Operasional tool katalog | Operasional tool DQ & pipeline gate |
| ESDP / Cloudera | Menghasilkan metadata teknis & operasional | Menjalankan quality gate |

## 8. Keterkaitan dengan Blueprint
- **3.1–3.3:** A2–A4, B2–B4. **3.4 / 5.3–5.4:** A6, B6–B7.
- **6 (roadmap) — usulan tema:**
  - **Tahun 1 — Fondasi:** harmonisasi glossary & Kamus Data untuk 5 shared dimension; federasi Dataedo–Atlas; daftar CDE dan metrik; gate di staging/bronze.
  - **Tahun 2 — Layanan:** lineage end-to-end; gate silver/gold; manajemen isu ber-SLA; data contract di katalog; monitoring freshness.
  - **Tahun 3 — Optimasi:** metadata operasional otomatis, skor kualitas tampil di katalog untuk self-service, evaluasi katalog enterprise tunggal.

## 9. Pertanyaan terbuka untuk BPJS
1. Sejauh mana Dataedo dipakai (skema tercakup, pengguna), dan apakah glossary disimpan di Dataedo?
2. Apakah Atlas aktif mencatat lineage untuk semua job di Cloudera?
3. Tool DQ otomatis apa yang dimaksud (D2 hlm. 28)? Talend Data Quality atau lainnya?
4. Apakah daftar CDE, metrik, dan skala indeks D1 sudah ditetapkan dan dipakai?
5. Apa isi Dashboard Kualitas Data dan siapa penggunanya?
6. Bagaimana alur perbaikan bad data di kantor cabang saat ini, dan berapa backlog-nya?
7. Apakah ada target freshness atau SLA data untuk dataset di platform?
8. Siapa yang berwenang mengubah definisi elemen data?

## 10. Poin diskusi (ikpu)
- [ ] Sepakati Opsi B (federasi Dataedo + Atlas) untuk metadata, dengan evaluasi katalog tunggal di Tahun 3.
- [ ] Sepakati Opsi D (definisi tunggal, eksekusi berlapis) untuk kualitas data.
- [ ] Tetapkan CDE awal pada 5 shared dimension sebagai pilot DQ dan harmonisasi Kamus Data.
- [ ] Sepakati elemen wajib metadata (A6) dan parameter wajib DQ (B7) tanpa nilai.
- [ ] Validasi skor, terutama MK-5, KD-2, dan KD-6 yang bertanda [perlu konfirmasi].

## Lampiran — Diagram
![[15-Metadata-Kualitas.excalidraw]]
