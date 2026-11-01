---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Penyimpanan Data + Dokumen & Konten
sesi: 8
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.3, 5.4, 6]
status: draft-diskusi
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, D3 status kapabilitas (D3-01), D4 Q&A WA 31 Okt 2026 (kebijakan Data Hub), D5 dokumen teknis API Kemnaker (RESTRICTED, hanya temuan), R1 rekaman diskusi 27 Okt 2026, PP 33/2026, 11-Master-Data.md, 12-Integrasi.md, 13-Arsitektur-Data.md, 14-Pemodelan-Data.md, 15-Metadata-Kualitas.md, 16-Keamanan-Data.md, 17-Tata-Kelola-Data.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 90 = cetak '- 89 -')."
---

# Sesi 8 — Penyimpanan Data + Dokumen & Konten

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI. **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (as-is). **D3-01** = slide "Arsitektur Current State dan Kapabilitas Pendukung" (status RAG). **D4** = Q&A WhatsApp 31 Okt 2026. **D5** = dokumen teknis API Kemnaker (RESTRICTED; hanya temuan, tanpa kredensial, alamat, atau nama host). **R1** = ekstraksi rekaman 27 Okt 2026, dirujuk [P hh:mm] / [S hh:mm]. "hlm." = halaman PDF.

> **Batas ruang lingkup (aturan keras).** Sesi ini **hanya** menilai apa yang dicakup platform data (ESDP): (1) penyimpanan di klaster Cloudera — HDFS, Kudu, Parquet — pada zona staging | lake | DWH (EDW) | data mart/serving, serta penyimpanan serving Data Hub; (2) operasi atas penyimpanan itu — kapasitas, backup/DR (termasuk DRC), retensi/arsip/tiering di dalam platform, parameter SLA kinerja dan freshness; (3) dokumen dan konten **sejauh diingest, disimpan, atau dilayani platform** — file tak terstruktur/semi-terstruktur di lake (scan, PDF, lampiran klaim, file Excel laporan iuran) dan pertukaran file SFTP lewat Data Hub, berikut metadata, klasifikasi, dan retensinya di platform. **Di luar lingkup dan tidak dinilai:** database aplikasi sumber (Oracle/SMILE, MariaDB, dsb.), standby database replikasi di sisi sumber, ECM/DMS, file server, dan backup tape korporat di luar platform. Komponen itu hanya disebut sebagai **antarmuka** (sumber upstream atau tujuan rujukan).

> **Kerangka sesi.** Konsisten dengan sesi 1–7: ESDP dibangun dengan Opsi B di atas Cloudera CDP on-prem eksisting; zona staging | lake | DWH | serving dipetakan ke landing/bronze/silver/gold ([[13-Arsitektur-Data]]). Integrasi adalah komponen ESDP: upstream = sumber → ESDP (CDC Qlik Replicate, ETL Talend); downstream = ESDP → pengguna/mitra melalui Data Hub (DMZ, B2B, 3scale REST/JSON atau SFTP; D4). SMILE adalah core system (SoR) di luar ESDP. Kontrol berbasis klasifikasi Biasa/Terbatas/Rahasia ([[16-Keamanan-Data]]); kepemilikan kebijakan retensi pada data owner, disusun hub tata kelola, dieksekusi custodian ([[17-Tata-Kelola-Data]] §13). Blueprint menetapkan **parameter** (RPO/RTO, masa retensi per kelas, ambang kapasitas, freshness), bukan nilainya.

## 1. Ringkasan
- **Penyimpanan platform berfungsi, tetapi satu lingkungan dan tanpa siklus hidup.** CDC masuk ke Kudu (diperbarui per jam), lalu dibentuk Parquet (≥1–2 jam) di atas HDFS; data mart sebagian besar juga Kudu/Parquet hasil job Talend (R1 [S 00:17:55–00:19:21; S 00:29:19–00:31:48]). D2 mencatat data lake dan DWH dikelola dalam satu environment, dengan risiko dampak serangan yang lebih besar (D2 hlm. 21).
- **DR platform adalah konflik sumber utama.** D3-01 menggambar "DRC (Active)" dengan 20 node total. R1 menyatakan komponen Cloudera di DRC **belum ada**: hanya tempat/rack dan uji coba satu mesin; hubungan DC–DRC belum didefinisikan (R1 [S 00:35:03–00:37:39]). D2 hanya menyebut fasilitas DC/DRC colocation Tier III (D2 hlm. 50). Skor DR disusun dengan asumsi R1 [perlu konfirmasi].
- **Backup, retensi, dan arsip di platform tidak terbukti.** D1 mengatur backup/recovery dan archiving database oleh Unit Operasional TI dengan profiling oleh UMD (Lampiran II B.2, hlm. 90–91), tetapi berorientasi database aplikasi. D3-01 menandai Retention Policy merah dan Archive kuning; Restore Testing tanpa status. Kebijakan retensi tidak ada (R1 [P 01:36:45–01:38:45]).
- **Freshness ada dalam praktik, belum sebagai SLA.** Frekuensi naik dari harian → 6 jam → paling cepat 1 jam (R1 [S 00:03:54–00:08:24]); streaming belum ada (R1 [S 00:04:21–00:04:47]). Tidak ada target freshness/kinerja tertulis per produk data.
- **Konten tak terstruktur: kapabilitas dinyatakan, belum dipakai.** D2 menandai "Unstructured Data" di data lake sebagai kapabilitas yang belum digunakan operasional (D2 hlm. 20). Pertukaran file bulk dengan K/L memakai SFTP terenkripsi AES-256 dengan validasi semi-otomatis (D2 hlm. 21; R1 [P 01:41:27–01:43:49; P 01:50:13–01:50:27]).
- **Skor usulan: Penyimpanan + Dokumen & Konten (lingkup ESDP) ±1,5** (13 aspek, rentang 1–2).
- **Rekomendasi (usulan, berbasis perbandingan):** **Opsi B — ketahanan bertingkat berbasis kelas data.** Zona serving/gold dan silver kritikal direplikasi ke klaster DRC warm-standby; bronze/staging dipulihkan dari sumber atau snapshot; tiering panas–hangat–dingin di dalam platform dengan jadwal retensi per kelas. Konten tak terstruktur masuk lake hanya jika dibutuhkan analitik/serving, selebihnya disimpan sebagai metadata + rujukan ke sistem dokumen di luar platform.

---

## 2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Staging | Data hasil ETL disimpan dulu di Data Staging (stand by database) sebelum ke lake | D2 hlm. 21 | Standby database sisi sumber di luar lingkup; hanya antarmuka upstream |
| Lake (bronze) | Cloudera, HDFS; menampung terstruktur dan tak terstruktur | D2 hlm. 21; R1 [S 00:02:33–00:02:48] | Unstructured belum operasional (D2 hlm. 20) |
| Silver/DWH | Tabel CDC di Kudu (upsert, per jam) → Parquet (≥1–2 jam) untuk query kompleks | R1 [S 00:17:55–00:19:21; S 00:29:19–00:30:41] | Dua salinan per tabel; kebijakan pemangkasan tidak diketahui |
| Data mart / gold | Sebagian besar Kudu/Parquet di Cloudera, hasil Talend; banyak cube finance | R1 [S 00:21:36–00:21:46; S 00:31:16–00:31:48] | Ukuran dan pemakaian per mart tidak diketahui |
| Isolasi lingkungan | Data Lake dan DWH dalam satu environment | D2 hlm. 21 | D2 menilainya sebagai risiko |
| Serving Data Hub | Belum dibangun; target DMZ, B2B, 3scale/SFTP | D4; D3-01 (API Management merah) | Penyimpanan serving belum dirancang |
| Kapasitas | Volume DWH/kapasitas belum dibahas; BPJS fokus data dulu | R1 [P 01:30:27–01:30:45] | Proyeksi peserta hingga 70 juta (D2 hlm. 37) relevan untuk rencana kapasitas |
| Backup platform | Backup off-site tape korporat ada, restore testing berkala belum | D2 hlm. 50 | Itu tingkat infrastruktur korporat (antarmuka); backup khusus HDFS/Kudu tidak diketahui [perlu konfirmasi] |
| DRC | D3-01: "DRC (Active)", 20 node; R1: Cloudera di DRC belum ada | D3-01; R1 [S 00:35:03–00:37:39] | **Konflik** — §8 |
| Retensi / arsip | Retention Policy merah, Archive kuning; kebijakan retensi tidak ada; data peserta 40+ tahun | D3-01; R1 [P 01:36:45–01:38:45; P 00:39:35–00:39:49] | Tiering tidak ada |
| Freshness | Paling cepat 1 jam; streaming belum ada | R1 [S 00:03:54–00:04:47] | Tidak ada SLA tertulis |
| Konten di lake | File Excel laporan iuran dari perusahaan diproses tim | R1 [P 00:05:38–00:05:49] | Jalur masuk ke platform tidak jelas [perlu konfirmasi] |
| Pertukaran file | SFTP bulk untuk PKS K/L; AES-256, key exchange per mitra, dikelola tim operasional; validasi semi-otomatis | D2 hlm. 21; R1 [P 01:41:27–01:43:49; P 01:50:13–01:50:27] | Kasus Kemnaker: salinan lokal di sisi mitra tanpa retensi (D5) |
| Monitoring | Sistem monitoring ada, tanpa pemetaan perangkat–aplikasi | D2 hlm. 50 | Monitoring khusus klaster tidak diketahui |

## 3. Kondisi yang seharusnya (D1)
- **Ps. 39 (hlm. 34–36):** Unit Operasional TI mengelola kapasitas (infrastruktur, SDM, finansial): dokumen perencanaan kapasitas, pemantauan, penambahan/penyesuaian kapasitas, dan **housekeeping**.
- **Ps. 48 + Lampiran II B (hlm. 39, 89–91):** UPTI melaksanakan penyimpanan dan operasional data: pengelolaan teknologi database (evaluasi kinerja, skalabilitas, volume/kecepatan) dan operasional database: analisis kebutuhan storage sesuai kaidah kapasitas; **backup dan recovery oleh Unit Operasional TI**; **archiving** — full backup lalu pindah ke storage yang ditetapkan; **UMD melakukan profiling kebutuhan archiving** berdasarkan intensitas akses, siklus hidup data, dan kinerja; akses data arsip sesuai matriks kontrol akses UMD; monev intensitas akses minimal setahun sekali.
- **Ps. 29 (hlm. 26):** kebijakan operasional mencakup pengelolaan kapasitas, manajemen ketersediaan dan keberlangsungan, serta penyimpanan dan operasional data.
- **Lampiran I DWBI (hlm. 76–79):** arsitektur DWBI/big data mencakup data terstruktur dan tak terstruktur, staging area sebagai penyimpanan sementara, data warehouse/data mart, dan publikasi ke BI.
- **Lampiran III A.2 (hlm. 93–94):** kontrol per klasifikasi memuat **backup data** (Biasa, Terbatas) dan kontrol akses pada level database atau **storage**.
- **Lampiran III (hlm. 96–97):** penghapusan informasi pada media yang tidak digunakan/dibuang.
- **Lampiran III (hlm. 105–106):** pemisahan environment development/testing/staging/production; Unit Operasional TI memegang staging dan production DWBI.
- **Yang tidak diatur D1:** RPO/RTO per kelas data, DR khusus platform analitik, jadwal retensi per kelas, tiering, SLA freshness, dan pengelolaan konten tak terstruktur.
- **Latar belakang:** PP 33/2026 Ps. 75 (dokumen kebijakan masa retensi; berlaku 16 Jan 2027) [perlu konfirmasi].

## 4. Tabel penilaian — Penyimpanan + Dokumen & Konten (lingkup ESDP)

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal.

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2/D3/R1/D4/D5) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| PS-1. Arsitektur zona penyimpanan | Zona staging/bronze/silver/gold terpisah dengan aturan masuk-keluar; isolasi lingkungan | Lampiran I DWBI (hlm. 76–79) | Zona ada secara fungsi; lake dan DWH satu environment (D2 hlm. 21) | Isolasi & aturan zona | 2 |
| PS-2. Standar format & teknologi penyimpanan | Pola baku Kudu (upsert) vs Parquet (analitik) vs HDFS mentah; konvensi partisi, kompaksi | Lampiran II B.1 (hlm. 89–90) | Pola Kudu→Parquet berjalan (R1 [S 00:17:55–00:19:21]); standar tertulis tidak ada; redundansi Kudu/Talend dipertanyakan (R1 [P 00:28:43–00:28:55]) | Standar tertulis | 2 |
| PS-3. Manajemen kapasitas platform | Rencana kapasitas per zona, ambang, proyeksi pertumbuhan | Ps. 39 (hlm. 34–36) | Volume/kapasitas belum dibahas (R1 [P 01:30:27–01:30:45]) | Rencana & ambang | 2 (batas kebijakan) |
| PS-4. Backup & restore platform | Snapshot HDFS, backup metadata (Hive Metastore, Atlas, Ranger), uji restore berkala | Lampiran II B.2 (hlm. 90); Lampiran III A.2 (hlm. 93–94) | Backup khusus klaster tidak terbukti; restore testing berkala belum ada di tingkat korporat (D2 hlm. 50) | Backup & uji restore | 1 [perlu konfirmasi] |
| PS-5. DR / DRC platform | RPO/RTO per kelas; klaster DR; replikasi; uji failover | Ps. 29 (hlm. 26) manajemen keberlangsungan | Cloudera di DRC belum ada (R1); slide menyatakan aktif (D3-01) | Klaster DR & parameter | 1 [perlu konfirmasi] |
| PS-6. SLA freshness & kinerja | Target freshness dan kinerja query per produk data; dipantau | Tidak ada | Freshness 1–2 jam dalam praktik (R1); tanpa SLA | SLA tertulis | 2 |
| PS-7. Retensi, arsip & tiering di platform | Jadwal retensi per kelas per zona; tiering panas–hangat–dingin; pemusnahan terbukti | Archiving database (hlm. 90–91), bukan platform | Retention Policy merah, Archive kuning (D3-01); kebijakan tidak ada (R1) | Jadwal & tiering | 1 |
| PS-8. Housekeeping zona sementara | Pembersihan staging/landing, versi lama Kudu/Parquet, file kecil | Ps. 39(3)c housekeeping (hlm. 36) | Tidak ada bukti | Prosedur | 1 |
| PS-9. Penyimpanan serving Data Hub | Store serving terpisah di DMZ, hanya produk gold sesuai PKS, retensi cache | — | Belum dibangun (D4; D3-01) | Desain | 1 |
| PS-10. Konten tak terstruktur di lake | Zona/area konten dengan konvensi path, format, metadata teknis | Lampiran I (hlm. 77) mencakup data tak terstruktur | Kapabilitas belum operasional (D2 hlm. 20); file Excel iuran diproses manual (R1) | Pola ingest konten | 1 |
| PS-11. Pertukaran file via SFTP | Satu jalur (Data Hub), enkripsi, checksum, validasi otomatis, log | Ps. 66 (hlm. 50–51); Lampiran III A.2 | SFTP AES-256, key exchange per mitra; validasi semi-otomatis (R1); bulk K/L (D2 hlm. 21); salinan lokal mitra tanpa retensi (D5) | Otomasi & retensi | 2 |
| PS-12. Metadata, klasifikasi & retensi konten | Setiap file di lake terdaftar di katalog dengan owner, klasifikasi, label PDP, masa retensi | Katalog Ps. 31 (hlm. 29) | Katalog/lineage merah (D3-01); konten belum terkatalog | Pendaftaran konten | 1 |
| PS-13. Monitoring operasional platform | Pemantauan klaster, job, storage, freshness terhubung ke insiden | Ps. 34–37 (hlm. 31–32) | Monitoring umum ada tanpa pemetaan aplikasi (D2 hlm. 50) | Monitoring per produk | 2 |

**Rata-rata Penyimpanan + Dokumen & Konten: ±1,5** (19/13).

## 5. Opsi ketahanan dan siklus hidup penyimpanan platform

| Opsi | Deskripsi |
|---|---|
| A. Backup-only | Tidak ada klaster DRC; snapshot HDFS + backup metadata ke storage off-site; pemulihan dengan membangun ulang klaster dan memuat ulang dari sumber |
| B. Ketahanan bertingkat berbasis kelas data | Klaster DRC warm-standby berkapasitas lebih kecil; replikasi terjadwal untuk gold/serving dan silver kritikal; bronze/staging dipulihkan dari sumber atau snapshot; tiering panas–hangat–dingin dan retensi per kelas |
| C. Active-active penuh | Dua klaster setara (DC dan DRC) dengan replikasi seluruh zona; workload dibagi |

**Perbandingan (++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria | A | B | C |
|---|---|---|---|
| Kemampuan memenuhi RPO/RTO ketat untuk Data Hub (B2B) | −− | + | ++ |
| Biaya lisensi & perangkat (Cloudera per tahun, R1) | ++ | 0 | −− |
| Kesesuaian dengan kondisi DRC saat ini (rack, belum ada komponen) | + | + (bertahap) | −− |
| Kompleksitas operasi | + | 0 | −− |
| Kesesuaian dengan kontrol per klasifikasi (D1 Lampiran III) | − | ++ | + |
| Dukungan retensi 40+ tahun dan tiering | 0 | ++ | 0 |
| Kesiapan tim | + | 0 | − |

**Rekomendasi (usulan): Opsi B.** Tidak semua zona perlu dipulihkan secepat yang lain: bronze dapat dibangun ulang dari sumber, sedangkan gold dan serving Data Hub menanggung komitmen ke mitra B2B. Opsi A tidak memadai setelah Data Hub melayani PKS. Opsi C mahal dan tidak sejalan dengan kondisi DRC saat ini menurut R1. Opsi B dijalankan bertahap: Tahun 1 snapshot + backup metadata + uji restore (titik awal Opsi A); Tahun 2 klaster DRC warm-standby untuk gold/serving.

**Sub-pilihan konten (dokumen di platform).**

| Opsi | Deskripsi | Plus | Minus |
|---|---|---|---|
| K1. Semua file disalin ke lake | Scan/PDF/lampiran klaim diingest utuh | Analitik/AI langsung | Duplikasi data pribadi; beban storage dan retensi |
| K2. Metadata + rujukan | Platform menyimpan metadata dan rujukan ke sistem dokumen di luar platform | Minimisasi; storage kecil | Bergantung pada ketersediaan sistem luar |
| K3. Hibrida berbasis tujuan | K2 sebagai default; file disalin hanya bila ada kasus analitik/serving yang disetujui owner | Seimbang; sejalan minimisasi | Butuh proses persetujuan |

**Rekomendasi konten: K3.** File Excel laporan iuran dan file SFTP mitra wajib diingest (dibutuhkan proses); scan/lampiran klaim hanya metadata + rujukan kecuali ada kasus yang disetujui.

## 6. Parameter wajib (tanpa nilai)

| Kelompok | Parameter yang harus ditetapkan | Pemilik (usulan, lihat [[17-Tata-Kelola-Data]]) |
|---|---|---|
| Ketahanan | RPO dan RTO per kelas data dan per zona; frekuensi snapshot; frekuensi uji restore & failover; cakupan backup metadata | Custodian (Ops TI) dengan owner domain |
| Kapasitas | Ambang pemakaian storage per zona; ambang peringatan/eskalasi; horizon proyeksi; pemicu penambahan node | Ops TI / tim platform ESDP |
| Retensi & tiering | Masa retensi per kelas (Biasa/Terbatas/Rahasia × kategori data); pemicu awal retensi; batas panas/hangat/dingin; metode pemusnahan & bukti | Data owner (A), hub (R), custodian (eksekusi) |
| Freshness & kinerja | Target freshness per produk data/zona; target latensi query; target ketersediaan serving Data Hub; jendela pemeliharaan | Owner produk data |
| Konten | Jenis file yang boleh diingest; ukuran maksimum; format; atribut metadata wajib; masa simpan file SFTP masuk/keluar | Hub tata kelola + OKI |
| Housekeeping | Umur maksimum data staging/landing; versi Parquet yang disimpan; jadwal kompaksi | Tim platform ESDP |

## 7. Retensi, arsip, dan tiering di dalam platform
- **Prinsip:** retensi diturunkan dari kelas data dan kebijakan owner ([[17-Tata-Kelola-Data]] §13), dieksekusi per zona. Bronze/staging menyimpan hanya selama dibutuhkan untuk reproses; silver/gold menyimpan riwayat sesuai kebutuhan bisnis; serving Data Hub hanya cache.
- **Tiering (konsep):** panas (Kudu, data aktif) → hangat (Parquet/HDFS terkompresi) → dingin (arsip di storage berbiaya rendah, akses dengan persetujuan sesuai matriks UMD, D1 hlm. 91).
- **Retensi jangka panjang 40+ tahun** (R1): fokus pada format terbuka (Parquet) dan metadata katalog agar data tetap terbaca lintas generasi platform.
- **Kontrol klasifikasi:** label Rahasia/PDP mengikuti data saat dipindah tier; enkripsi dan kebijakan Ranger tetap berlaku ([[16-Keamanan-Data]]).
- **Profiling arsip** oleh UMD (intensitas akses, siklus hidup, kinerja) sesuai D1 hlm. 90–91, diterapkan ke tabel platform, bukan hanya database aplikasi.

## 8. Konflik DRC dan backup

| Sumber | Pernyataan |
|---|---|
| D3-01 | DRC (Active), 20 node total (koordinator, worker, MDM engine) |
| D2 hlm. 50 | Fasilitas DC dan DRC colocation Tier III; backup off-site tape tanpa restore testing berkala |
| R1 [S 00:35:03–00:37:39] | Cloudera di DRC belum ada komponennya; hanya rack, uji coba 1 mesin; hubungan DC–DRC belum didefinisikan |

Tafsiran sementara: "DRC (Active)" di D3-01 kemungkinan merujuk ke **situs/klaster utama yang berjalan di gedung DRC** atau ke rencana, bukan klaster pemulihan [perlu konfirmasi]. Sampai terkonfirmasi, Blueprint mengasumsikan **tidak ada salinan pemulihan klaster** (PS-5 = 1).

## 9. Dokumen & konten di platform
- **Masuk (upstream):** file Excel laporan iuran (R1), file bulk SFTP dari K/L (D2 hlm. 21), dan potensi scan/PDF/lampiran klaim dari aplikasi layanan (sumber di luar lingkup).
- **Penyimpanan:** area konten di bronze dengan konvensi path (domain/sumber/tanggal), checksum, dan file manifest; versi terstruktur hasil ekstraksi masuk silver.
- **Keluar (downstream):** SFTP via Data Hub untuk PKS bulk; file keluar dibuat dari gold, dicatat per PKS, dan dihapus dari area keluar sesuai parameter.
- **Metadata & klasifikasi:** setiap file/kumpulan terdaftar di Atlas/Dataedo dengan owner, klasifikasi, label PDP, sumber/PKS, masa retensi ([[15-Metadata-Kualitas]]).
- **Kasus Kemnaker (D5):** salinan lokal di sisi mitra tanpa retensi menunjukkan bahwa masa simpan file keluar harus diatur juga di dokumen teknis PKS ([[17-Tata-Kelola-Data]] §9).

## 10. Batas peran

| Peran | Lingkup di sesi ini |
|---|---|
| Unit Operasional TI (custodian) | Backup/restore, kapasitas, DR, eksekusi tiering & pemusnahan (D1 hlm. 90) |
| Tim platform ESDP | Standar format, housekeeping, monitoring job dan freshness |
| UMD (hub) | Profiling arsip, matriks akses arsip (D1 hlm. 90–91), pendaftaran konten di katalog |
| Data owner | Masa retensi, target freshness, RPO/RTO kelas data domainnya |
| OKI | Kontrol enkripsi, SFTP, akses storage |
| Pemilik sistem sumber / ECM | Di luar lingkup; antarmuka sumber dan rujukan dokumen |

## 11. Keterkaitan dengan Blueprint
- **3.1–3.3:** bagian 2–4. **3.4 / 5.3–5.4:** bagian 5–10.
- **6 (roadmap) — usulan tema "Platform yang dapat dipulihkan dan berumur panjang":**
  - **Tahun 1 — Fondasi:** klarifikasi DRC; snapshot HDFS + backup metadata + uji restore pertama; rencana kapasitas dan ambang; parameter freshness untuk produk data prioritas; housekeeping staging; pola ingest konten (Excel iuran, SFTP).
  - **Tahun 2 — Layanan:** klaster DRC warm-standby untuk gold/serving Data Hub; tiering dan retensi per kelas berjalan; SFTP lewat Data Hub dengan validasi otomatis; konten terdaftar di katalog.
  - **Tahun 3 — Optimasi:** uji failover rutin; otomasi tiering/pemusnahan berbasis tag; evaluasi pemisahan environment lake/DWH; pemantauan SLA per produk data.

## 12. Pertanyaan terbuka untuk BPJS
1. Apa arti "DRC (Active), 20 node" di slide dibandingkan pernyataan bahwa Cloudera belum ada di DRC?
2. Apakah ada snapshot/backup HDFS, Kudu, dan metadata klaster? Kapan restore terakhir diuji?
3. Berapa kapasitas terpasang, pemakaian, dan pertumbuhan per zona?
4. Apakah ada target freshness/kinerja tertulis untuk produk data atau laporan tertentu?
5. Jenis dokumen/konten apa yang saat ini masuk ke lake, dan lewat jalur apa?
6. Berapa lama file SFTP masuk/keluar disimpan, dan siapa yang menghapusnya?

## 13. Poin diskusi (ikpu)
- [ ] Tegaskan batas lingkup: hanya penyimpanan dan konten di ESDP; database sumber dan ECM sebagai antarmuka.
- [ ] Selesaikan konflik DRC (D3-01 vs R1) sebelum memfinalkan skor PS-5.
- [ ] Sepakati Opsi B (ketahanan bertingkat berbasis kelas) dan sub-opsi konten K3.
- [ ] Sepakati daftar parameter wajib (RPO/RTO, retensi per kelas, ambang kapasitas, freshness) dan pemiliknya, tanpa nilai.
- [ ] Jadikan uji restore klaster sebagai quick win Tahun 1.
- [ ] Pastikan masa simpan file SFTP keluar masuk dokumen teknis PKS (kaitan Kemnaker).

## Lampiran — Deskripsi diagram (gaya Excalidraw)
Kiri: kotak abu-abu putus-putus **"Di luar lingkup: DB sumber (SMILE/Oracle/MariaDB), ECM/DMS"** dengan panah upstream (CDC/ETL, file Excel, SFTP masuk) ke klaster **ESDP (Cloudera, DC)** berisi empat pita: **Staging/Landing → Bronze (HDFS + area konten) → Silver (Kudu → Parquet) → Gold/Data Mart**. Di bawah klaster: tiga tier **Panas | Hangat | Dingin** dengan panah tiering dan ikon jam retensi per kelas. Kanan: **Data Hub (DMZ) serving store → 3scale API / SFTP keluar → Mitra**. Bawah: klaster **DRC warm-standby** menerima panah replikasi hanya dari Gold dan Serving (garis tegas) dan snapshot dari Bronze (garis putus-putus), diberi tanda tanya "status DRC [perlu konfirmasi]". Pojok kanan atas: kotak parameter "RPO/RTO · retensi per kelas · ambang kapasitas · freshness".
