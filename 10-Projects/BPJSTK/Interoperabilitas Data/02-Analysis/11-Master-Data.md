---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Master & Reference Data
sesi: 1
blueprint: [3.4, 5.2, 5.3]
status: draft-diskusi
acuan_internal: DAMA-DMBOK v2, bab Reference & Master Data
sumber: [D3 paparan MDM & model konseptual, D4 kebijakan API Data Hub, D5 dokumen teknis API Kemnaker, R1 rekaman diskusi, D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, analisis_dokumen_bpjs.md, bpjstk_tender_inventory.csv]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 88 = cetak '- 87 -')."
---

# Sesi 1 — Master & Reference Data

## 1. Ringkasan
- **Gap terbesar ada di antara kebijakan dan praktik.** D1 mewajibkan penambahan/pembaruan master data secara terpusat di *satu master data repository* (D1 hlm. 88), tetapi D2 mencatat bahwa pengelolaan master data masih manual, belum diatur SOP, dan fungsinya belum didefinisikan (D2 hlm. 24).
- **Tidak ada tool MDM.** D2 hlm. 24 mencatat pengelolaan manual. Inventaris tender juga tidak memuat pengadaan MDM; yang ada lisensi Cloudera/Dataedo/Qlik dan pengadaan infrastruktur data analitik.
- **Big data (Cloudera) tidak mengacu ke master data**, sehingga datanya "belum tentu" *single source of truth* (D2 hlm. 24). Batas SoR–MDM–ESDP–Cloudera belum didefinisikan di dokumen mana pun. "ESDP" tidak muncul di D1 maupun D2.
- **Skor usulan rata-rata rendah (1–2).** Hampir semua aspek baru berupa kebijakan tanpa bukti praktik.
- **Usulan awal untuk didiskusikan:** mulai dengan pola *registry → consolidation* untuk Peserta/TK dan PK/BU, lalu bergerak ke *coexistence*. Reference data dikelola dengan pola *centralized*.

## 2. Tabel penilaian

Aturan skor: 1 = tidak ada/ad hoc; 2 = kebijakan ada tetapi praktik belum terbukti (batas atas jika hanya ada kebijakan); 3 = praktik ada sebagian/terdokumentasi; 4 = terkelola & terukur; 5 = teroptimasi. Setiap skor harus punya bukti.

| Aspek                                                                                            | Praktik yang diharapkan                                                                      | Kebijakan BPJS (D1)                                                                                                                                                                                                      | Kondisi saat ini (D2)                                                                                                                                                                                             | Gap                                                                                                                            | Skor usulan (1-5)                  |
| ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------- |
| Identifikasi entitas master (peserta/TK, PK/BU, wilayah, kantor cabang, program, kode referensi) | Daftar entitas master & referensi yang disepakati, lengkap dengan definisi dan atribut kunci | Kebutuhan & lingkup MD&DR memuat entitas & elemen data, pemetaan ke sistem berjalan, dan analisis variansi, disusun bertahap sesuai prioritas (D1 hlm. 87–88)                                                            | Data Building Block mencakup Data Tenaga Kerja, Data Primer/Sekunder Peserta, Data Pemberi Kerja/Badan Usaha, Data Profil Kantor Daerah, Data Mitra Perbankan (D2 hlm. 16). Belum ada daftar resmi entitas master | Belum ada katalog entitas master yang ditetapkan. Wilayah, program, dan kode referensi tidak terlihat sebagai master/referensi | 2 → **3** (31 Okt 2026): model konseptual Kepesertaan 13 entitas berdefinisi (D3-04, D3-05); belum mencakup JKP (R1 [P 01:21:33–01:22:26]) |
| Kepemilikan & stewardship                                                                        | Owner bisnis & steward per entitas, RACI                                                     | Prinsip "penetapan kepemilikan" MD&DR (D1 hlm. 87). Unit Manajemen Data bertugas atas master data & data referensi (D1 hlm. 9, Ps. 8(3)). Kancab sebagai *data producer* (D1 hlm. 10). Peran *data liaison* (D1 hlm. 89) | Fungsi pengelola master data belum didefinisikan secara jelas (D2 hlm. 24). Kewenangan pengubahan elemen data tidak jelas (D2 hlm. 26). Data Steward baru muncul di ilustrasi target (D2 hlm. 23)                 | Owner/steward per entitas belum ditetapkan                                                                                     | 1                                  |
| Golden record & aturan matching/merge (NIK sebagai kunci, Dukcapil)                              | Aturan identifikasi, *match/merge/survivorship*, dan verifikasi ke sumber otoritatif         | Fungsi "pengelolaan identitas data" dan konsolidasi ke MD&DR (D1 hlm. 88). Contoh aturan validitas: NIK 16 digit numerik (D1 hlm. 84)                                                                                    | Tidak ada bukti aturan match/merge atau golden record. Integrasi Dukcapil tidak disebut di D2 [perlu konfirmasi]                                                                                                  | Aturan matching, *survivorship*, dan penanganan NIK ganda/kosong belum ada                                                     | 1 → **2** (31 Okt 2026): POC MDM dengan pembobotan sumber dan pengecekan kualitas (D3-02; R1 [P 01:25:45–01:26:50]); belum produksi |
| Sumber kebenaran / SoR per entitas                                                               | SoR ditetapkan per entitas/atribut                                                           | Single master data repository terpusat (D1 hlm. 88)                                                                                                                                                                      | SMILE adalah core system yang dipakai di seluruh direktorat (D2 hlm. 35). Ada aplikasi yang belum terintegrasi dengan SMILE (D2 hlm. 45). Big data belum tentu SSoT (D2 hlm. 24)                                  | SoR per entitas tidak terdokumentasi. Asumsi "SMILE = SoR Peserta/PK" [perlu konfirmasi]                                       | 1 → **2** (31 Okt 2026): sumber POC MDM teridentifikasi: SMILE Pendaftaran/Klaim, JMO, POM, Lapak Asik (D3-02); SoR per atribut belum ditetapkan |
| Distribusi/sinkronisasi ke konsumen                                                              | Layanan *publish/subscribe* atau API master data dengan SLA                                  | Fungsi "sharing" MD&DR (D1 hlm. 88). Sistem wajib menjaga konsistensi nilai master data (D1 hlm. 88). Publikasi melalui data liaison (D1 hlm. 89)                                                                        | Integrasi via ADG, Talend, Qlik Replicate, API (D2 hlm. 21). Mekanisme ini bersifat replikasi data, bukan distribusi master data. Big data tidak mengacu master data (D2 hlm. 24)                                 | Belum ada layanan distribusi master data                                                                                       | 2                                  |
| Reference data / kode standar                                                                    | Kode referensi dikelola terpusat dengan *versioning* dan pemetaan ke standar eksternal       | MD&DR dikelola untuk dibagipakaikan dan dievaluasi minimal setahun sekali (D1 hlm. 30, Ps. 32). Kamus data memuat "Example of Valid Values" (D2 hlm. 25, sebagai standar)                                                | Perbedaan nama & definisi elemen data antar-skema di Kamus Data (D2 hlm. 26). Pengelolaan kode referensi (wilayah, program, dll.) tidak dibahas [perlu konfirmasi]                                                | Belum ada daftar kode referensi resmi dan pemiliknya                                                                           | 2                                  |
| Kualitas & deduplikasi                                                                           | Metrik keunikan/konsistensi master dipantau, deduplikasi berkelanjutan                       | Dimensi uniqueness & consistency, termasuk metrik konsistensi terhadap SMILE (D1 hlm. 84). MD&DR dipantau berkala (D1 hlm. 87)                                                                                           | Masih ditemukan duplikasi, inkonsistensi, dan format tidak valid. Perbaikan bad data di kancab belum optimal (D2 hlm. 28). Ada proses DQ as-is: profiling, cleansing (D2 hlm. 27)                                 | DQ berjalan, tetapi tidak terkait golden record. Tingkat duplikasi tidak terukur di dokumen [perlu konfirmasi]                 | 2                                  |
| Tools                                                                                            | Tool MDM (hub/registry) dan tool reference data                                              | Arsitektur MD&DR: identitas, create/update, sharing (D1 hlm. 88). Tidak menyebut produk                                                                                                                                  | Manual, tanpa tool MDM (D2 hlm. 24). Ilustrasi target berupa MDM tools (D2 hlm. 23). Tidak ada tender MDM di inventaris (Cloudera, Dataedo, Qlik saja)                                                            | Tidak ada tool                                                                                                                 | 1 → **2** (31 Okt 2026): mesin MDM dibangun internal dalam tahap POC (D3-02, D3-01) [perlu konfirmasi lokasi & status] |
| Proses/SOP                                                                                       | SOP create/update/retire, alur persetujuan perubahan, review berkala                         | Perubahan terkendali (D1 hlm. 87). Review minimal 1x/tahun (D1 hlm. 89; Ps. 32(3) hlm. 30)                                                                                                                               | Pengelolaan & penggunaan master data tidak diatur dalam SOP, sehingga evaluasi tidak dapat dilakukan (D2 hlm. 24)                                                                                                 | SOP belum ada                                                                                                                  | 1                                  |
| Implikasi integrasi Taspen/Asabri 2029                                                           | Identitas peserta lintas lembaga dapat dicocokkan, kode referensi selaras                    | Tidak disebut di D1. D1 hanya mengatur Kolaborasi & Pemanfaatan Data secara umum (D1 hlm. 9, definisi)                                                                                                                   | Tidak disebut di D2                                                                                                                                                                                               | Kebutuhan master data lintas lembaga belum terpetakan. Dasar dan jadwal 2029 [perlu konfirmasi]                                | – (tidak dinilai, belum ada bukti) |

## 3. Temuan utama
1. **Kebijakan sudah memadai di atas kertas.** D1 mengatur prinsip MD&DR (shared, kepemilikan, kualitas, perubahan terkendali), arsitektur (identitas, create/update, sharing), dan penggunaan terpusat di satu repository (D1 hlm. 30, 87–89). Isi ini mendekati praktik yang diharapkan.
2. **Praktik belum berjalan.** Master data dikelola manual, tanpa SOP, tanpa fungsi pengelola yang jelas, dan big data tidak mengacu master data (D2 hlm. 24). D2 sendiri menyatakan target MDM tool + Data Steward hanya sebagai ilustrasi (D2 hlm. 23).
3. **Akuntabilitas kabur.** Kewenangan mengubah elemen data tidak ditentukan secara jelas (D2 hlm. 26), sementara D1 menuntut kepemilikan ditetapkan (D1 hlm. 87).
4. **Masalah kualitas yang relevan untuk MDM sudah diakui.** D2 mencatat duplikasi dan inkonsistensi (D2 hlm. 28). D1 sudah menyediakan metrik keunikan dan konsistensi terhadap SMILE (D1 hlm. 84), tetapi capaian metrik tidak dilaporkan [perlu konfirmasi].
5. **Lanskap aplikasi terfragmentasi.** SMILE adalah core (D2 hlm. 35), tetapi sebagian aplikasi belum terintegrasi dengan SMILE (D2 hlm. 45) dan database beragam: Oracle, MariaDB, Yugabyte, SQL Server, PostgreSQL (D2 hlm. 37). Risiko salinan master data yang divergen tinggi.
6. **Metadata sudah ada tooling, master data belum.** Dataedo dan Apache Atlas (Cloudera) dipakai untuk metadata (D2 hlm. 26). Fondasi glossary/kamus data ini bisa dipakai ulang untuk definisi entitas master.

## 4. Opsi pola MDM

| Pola | Kelebihan | Kekurangan | Kecocokan awal BPJS TK |
|---|---|---|---|
| Registry | Invasif paling rendah. Indeks silang ID antar-sistem, cepat dimulai | Tidak ada golden record fisik. Kualitas tetap bergantung pada sumber | Tinggi sebagai langkah awal |
| Consolidation | Golden record untuk analitik/pelaporan. Tidak mengubah aplikasi operasional | Golden record tidak mengalir balik ke operasional | Tinggi, selaras dengan kebutuhan DWBI/big data (D1 hlm. 77) |
| Coexistence | Golden record dipublikasikan kembali ke sumber. Konsistensi meningkat bertahap | Butuh integrasi dua arah dan tata kelola perubahan yang matang | Target menengah |
| Centralized (transaction hub) | Paling sesuai teks D1 ("terpusat… repository tunggal", D1 hlm. 88). Satu titik create/update | Paling invasif: mengubah alur SMILE/kanal. Risiko tinggi pada core monolitik (D2 hlm. 37) | Rendah untuk Peserta/PK saat ini. Cocok untuk reference data |

**Usulan awal (untuk didiskusikan):** pendekatan hibrid bertahap.
- **Reference data:** *centralized* sejak awal (volume kecil, risiko rendah, langsung memenuhi D1 hlm. 88).
- **Peserta/TK dan PK/BU:** *registry → consolidation* dulu (golden record di sisi analitik, kunci NIK/identitas PK [perlu konfirmasi kunci PK]). Setelah SoR, owner, dan SOP disepakati, bergerak ke *coexistence*.
- **Alasan:** (a) belum ada SOP/owner (D2 hlm. 24), sehingga pola invasif belum siap tata kelolanya; (b) core monolitik dan database beragam (D2 hlm. 37) meningkatkan risiko pola *centralized* untuk entitas transaksional; (c) arah akhir tetap dapat menyesuaikan diri ke kewajiban repository tunggal di D1. Interpretasi "terpusat" (hub fisik vs. tata kelola terpusat) perlu dikonfirmasi ke Unit Manajemen Data.

**Diagram pola MDM hibrid bertahap & alur golden record (usulan):**

![[11-Master-Data-MDM.excalidraw]]

## 4A. Masukan Desain Golden Record (pembaruan 31 Okt 2026)

### 4A.1 Pola BPJS (D3-02, D3-03) dibandingkan desain yang kami rekomendasikan

| Elemen | Pola BPJS saat ini (POC) | Desain rekomendasi | Catatan |
|---|---|---|---|
| Sumber | JMO, POM, Lapak Asik, SMILE Pendaftaran dan SMILE Klaim (D3-02) | Sama, ditambah sumber otoritatif eksternal (Dukcapil untuk NIK, OSS untuk NIB) sebagai rujukan verifikasi, bukan sumber tulis | SMILE tetap SoR transaksi |
| Pencocokan & penggabungan | Pembobotan (scoring) per sumber dan pengecekan kualitas elemen (D3-02); Pencocokan dan DQ dalam satu lingkup Data Stewardship (D3-03) | Aturan match deterministik pada kunci kuat (NIK tervalidasi, NPP, NIB) lalu probabilistik pada atribut pendukung (nama, tanggal lahir, alamat); survivorship per atribut, bukan per record | Kasus ragu masuk antrean steward |
| Isi golden record | Contoh memuat Nama, Pekerjaan, Perusahaan **dan Saldo** (D3-02); BPJS menyatakan isi (dimensi saja atau juga fakta) masih konsep dan meminta masukan (R1 [P 01:26:52–01:27:29; P 01:32:39–01:33:45]) | Hanya atribut master; **saldo, iuran, dan klaim tidak masuk** | Saldo adalah fakta transaksi yang berubah tiap periode; menaruhnya di golden record mencampur dua siklus hidup |
| Penyimpanan | 360° Database; keluaran ke tabel data mart (D3-02, D3-03); MDM diletakkan setelah staging, sebelum DWH, sebagai bagian DWH (R1 [S 00:09:18–00:09:42]); golden record ditampung di wadah baru yang dapat diakses dari luar (R1 [P 01:25:45–01:26:50]) | Hub MDM terpisah dari data mart; data mart (gold ESDP) memakai golden ID sebagai kunci dimensi | Tampilan 360° = gabungan golden record + fakta dari gold, dibentuk di lapis serving |
| Pembaruan | Real time opsional dari aplikasi bisnis (D3-03) | Bertahap: batch harian/CDC (freshness mengikuti pipeline Qlik, R1 [S 00:01:56–00:07:23]) → near real-time untuk entitas yang dibutuhkan layanan | Target freshness = parameter |
| Distribusi | Zona Penyebaran Data via API ke Kepesertaan, Pelayanan, Eksternal (D3-03) | Sejalan; API diekspos melalui Data Hub/3scale (D4), bukan akses langsung ke database | Lihat 4A.5 |
| Stewardship | Kotak Data Stewardship di diagram (D3-03); penggabungan kasus per kasus lewat rapat (R1 [P 01:06:06–01:08:42]) | Steward per domain dengan alur keputusan tercatat dan SLA | Praktik saat ini belum terdokumentasi |

### 4A.2 Lingkup: yang masuk dan yang tidak masuk golden record

**Masuk golden record (entitas master inti):**
- **Individu** (orang, kunci NIK) dan **Peserta** (Individu yang terdaftar pada program; D3-05). Satu Individu dapat memiliki lebih dari satu kepesertaan.
- **Perusahaan** (badan usaha, potensi atau terdaftar), **Pemberi Kerja** (Perusahaan yang terdaftar sebagai PK, kunci NPP), **Unit Pemberi Kerja** (unit administrasi; D3-05), serta **Grup Perusahaan** sebagai hierarki.
- **Kantor** (kantor wilayah/cabang BPJS) dan entitas pendukung yang stabil (Agen, Pembina, Pelaksana Penempatan PMI) [perlu konfirmasi prioritas].
- **Reference data**: kode program, wilayah, jenis pekerjaan, sektor/KBLI, skala usaha, segmen peserta. Dikelola terpusat (pola *centralized*, §4).

**Tidak masuk golden record (fakta transaksi; tetap di gold/data mart ESDP):**
- Iuran, laporan upah, mutasi saldo, saldo program, piutang (D3-04).
- Kasus klaim, klaim manfaat, pembayaran klaim, beasiswa (D3-06).
- Relasi berwaktu yang bersifat peristiwa (Pendaftaran, HubunganKerja) disimpan sebagai fakta/histori di ESDP; golden record hanya memegang status relasi terkini dan rujukan ID [perlu konfirmasi kebutuhan layanan].

### 4A.3 Kunci resolusi identitas

| Entitas | Kunci utama | Kunci pendukung | Sumber validasi |
|---|---|---|---|
| Individu | NIK | Nama, tanggal lahir, nama ibu kandung [perlu konfirmasi atribut] | Dukcapil |
| Peserta | KPJ, kode TK, kode kepesertaan (R1 [P 00:34:11–00:34:55]) | NIK (melalui Individu), program | SMILE |
| Pemberi Kerja / Unit | NPP (R1 [P 00:34:11–00:34:55]) | NIB, NPWP badan, nama, alamat | SMILE; OSS (NIB) |
| Perusahaan | NIB | NPWP badan, nama | OSS / Kemnaker (D5 hlm. 11–12) |

**Kasus khusus Pemberi Kerja:**
- **Pindah peserta antar-PK/cabang:** saat ini kode kepesertaan berubah sementara kode TK dan nomor kartu dapat tetap; mutasi perorangan terjadi setiap hari (R1 [P 01:09:48–01:10:44]). Golden record Peserta memakai kode TK sebagai jangkar dan menyimpan crosswalk kode kepesertaan lama → baru.
- **Pindah PK (mutasi kantor/wilayah, ganti alamat):** golden ID tetap; perubahan disimpan sebagai histori atribut (SCD tipe 2 di ESDP, R1 [S 00:00:37–00:00:49]).
- **Merger / akuisisi / pemecahan:** saat ini diputuskan kasus per kasus (R1 [P 01:06:06–01:08:42]). Usulan: catat sebagai peristiwa relasi (pendahulu → penerus) dengan tanggal efektif; NPP lama tidak dihapus, ditandai *retired* dengan rujukan ke penerus; fakta historis tetap menempel ke NPP asal.
- **Satu NIB dengan banyak NPP/unit:** dimodelkan sebagai hierarki Perusahaan → Pemberi Kerja → Unit Pemberi Kerja, bukan sebagai duplikat.

### 4A.4 Opsi desain golden record

| Opsi | Deskripsi |
|---|---|
| A. Golden record "360°" lebar (pola POC saat ini) | Satu record memuat atribut master dan ringkasan fakta (mis. saldo), langsung ke data mart |
| B. Golden record master-only + tampilan 360° di serving | Hub hanya memuat entitas master dan crosswalk ID; fakta tetap di gold ESDP; tampilan 360° dirakit di serving/API |
| C. Registry saja | Hub hanya menyimpan crosswalk ID antarsumber; atribut tetap di sumber |

| Kriteria | A | B | C |
|---|---|---|---|
| Kejelasan siklus hidup data (master vs transaksi) | −− | ++ | + |
| Konsistensi dengan shared dimension ESDP ([[14-Pemodelan-Data]]) | − | ++ | + |
| Beban survivorship & pemeliharaan | −− | + | ++ |
| Nilai bagi layanan (tampilan lengkap pelanggan) | ++ | + | − |
| Kemudahan transisi dari POC | ++ | + | 0 |
| Risiko PDP (minimisasi data per konsumen) | − | + | + |

**Rekomendasi (usulan): Opsi B,** dengan Opsi C sebagai tahap awal untuk entitas yang belum siap (sejalan dengan jalur registry → consolidation → coexistence di §4). Kebutuhan layanan atas tampilan lengkap tetap terpenuhi melalui API serving yang menggabungkan golden record dan fakta, tanpa menyimpan fakta di hub.

### 4A.5 Distribusi
- Golden record didistribusikan melalui **API** yang diekspos di **Data Hub/3scale** (D4), baik ke aplikasi internal (Kepesertaan, Pelayanan) maupun mitra eksternal (D3-03).
- **Tidak ada akses langsung ke database hub** oleh aplikasi atau mitra; pola DB link seperti pada D5 hlm. 3–5 tidak direplikasi (lihat [[12-Integrasi]]).
- ESDP menerima golden ID dan atribut master melalui pipeline terkelola (CDC/batch) untuk membentuk shared dimension.

### 4A.6 Parameter yang harus ditetapkan (tanpa nilai)
1. Daftar entitas dan atribut golden record per entitas.
2. Prioritas sumber per atribut (bobot survivorship) dan aturan *tie-break*.
3. Aturan match deterministik dan probabilistik; ambang skor otomatis-gabung, antrean steward, dan tolak.
4. Atribut yang wajib diverifikasi ke sumber otoritatif dan frekuensi verifikasinya.
5. Aturan peristiwa PK: pindah, merger, pemecahan, penutupan; masa berlaku NPP *retired*.
6. Target freshness golden record per konsumen.
7. SLA keputusan steward dan jalur eskalasi.
8. Elemen yang boleh diekspos per kelas konsumen (internal, eksternal) dan aturan masking.
9. Retensi histori dan crosswalk ID.

## 5. Implikasi ke Blueprint (3.4, 5.2, 5.3)
- **SoR (SMILE dan aplikasi operasional):** tetap menjadi tempat transaksi dan entri awal. SoR ditetapkan per entitas/atribut dalam blueprint [perlu konfirmasi daftar SoR].
- **MDM:** satu-satunya tempat identitas, *match/merge*, golden record, dan cross-reference ID. Juga menjadi pemilik kode referensi. Bukan tempat transaksi dan bukan gudang analitik.
- **ESDP:** istilah ini tidak ada di D1/D2, sehingga peran dan cakupannya [perlu konfirmasi]. Usulan kerja: jalur distribusi/integrasi (API/event) yang menyalurkan golden record dan kode referensi ke konsumen, sesuai fungsi "sharing" (D1 hlm. 88).
- **Cloudera (big data/DWH):** konsumen master data. Wajib memakai kunci & dimensi dari MDM agar menutup temuan "big data tidak mengacu master data" (D2 hlm. 24), sesuai komponen MD&DR dalam arsitektur DWBI (D1 hlm. 77). Cloudera tidak boleh menjadi tempat pembentukan golden record secara de facto.
- **Metadata (Dataedo/Atlas):** menjadi sumber definisi entitas dan atribut master (D2 hlm. 26).

## 6. Pertanyaan terbuka untuk BPJS
1. Entitas apa yang secara resmi dianggap master data dan reference data? Apakah sudah ada hasil analisis lingkup MD&DR sesuai D1 hlm. 87–88?
2. Apa SoR per entitas (Peserta/TK, PK/BU, kantor, mitra, wilayah, program)? Apakah SMILE SoR tunggal untuk Peserta dan PK/BU?
3. Apa kunci identitas yang dipakai? Berapa proporsi NIK kosong/ganda/tidak valid? Bagaimana kunci PK/BU (mis. NPP) [perlu konfirmasi]?
4. Apakah sudah ada integrasi verifikasi NIK ke Dukcapil? Seperti apa mekanisme dan frekuensinya?
5. Siapa owner dan steward per entitas saat ini? Siapa yang berperan sebagai *data liaison* (D1 hlm. 89)?
6. Bagaimana tafsiran internal atas "repository tunggal terpusat" (D1 hlm. 88): hub fisik atau tata kelola terpusat?
7. Apa definisi dan cakupan ESDP, dan bagaimana relasinya dengan Talend/Qlik Replicate/API (D2 hlm. 21)?
8. Apa ruang lingkup, dasar hukum, dan data yang dipertukarkan dalam integrasi Taspen/Asabri 2029?
9. Apakah ada rencana pengadaan tool MDM/reference data di RKAT/ITMP 2025–2027?
10. Bagaimana tabel kode referensi dikelola saat ini, dan apakah ada standar eksternal yang wajib diikuti (mis. kode wilayah) [perlu konfirmasi]?

## 7. Poin diskusi (ikpu)
- [ ] Sepakati konvensi sitasi halaman (PDF vs. cetak) untuk semua catatan sesi.
- [ ] Validasi skor usulan, terutama skor 1 pada stewardship, golden record, SoR, tools, dan SOP.
- [ ] Putuskan apakah aspek Taspen/Asabri 2029 dinilai atau dibiarkan sebagai catatan sampai ada bukti.
- [ ] Uji usulan hibrid (reference centralized; Peserta/PK registry→consolidation→coexistence) terhadap teks D1 hlm. 88.
- [ ] Tetapkan definisi kerja ESDP sebelum menulis Blueprint 5.2/5.3.
- [ ] Prioritaskan pertanyaan 2, 3, 4, dan 6 untuk sesi dengan Unit Manajemen Data.
- [ ] Putuskan apakah posisi "Cloudera bukan pembentuk golden record" menjadi prinsip arsitektur di Blueprint 3.4.


## Pembaruan 2026-10-31

**Bukti baru (kondisi saat ini):**
- BPJS sedang menjalankan **POC MDM** yang dibangun internal: sumber JMO, POM, Lapak Asik, SMILE Pendaftaran/Klaim; penggabungan dengan pembobotan dan pengecekan kualitas; keluaran ke tabel data mart (D3-02; R1 [P 01:25:45–01:26:50]). Pernyataan D2 hlm. 24 ("tanpa tool") kini sebagian tidak berlaku.
- Model konseptual Kepesertaan memuat 13 entitas berdefinisi, termasuk pembedaan Perusahaan/Pemberi Kerja/Unit Pemberi Kerja dan Individu/Peserta (D3-04, D3-05). Model berusia ±3 tahun dan belum memuat JKP (R1 [P 01:21:33–01:22:26]).
- Risiko kepesertaan ganda yang berujung klaim ganda diakui (R1 [P 01:11:20–01:11:40]); volume ±100-an juta ID perorangan (R1 [P 01:09:16–01:09:30]) [perlu konfirmasi].
- Kunci identitas yang dipakai: KPJ, kode TK, NPP, kode kepesertaan (R1 [P 00:34:11–00:34:55]). Penggabungan PK diputuskan kasus per kasus (R1 [P 01:06:06–01:08:42]).
- Contoh golden record POC memuat Saldo (D3-02), yaitu fakta transaksi; lihat §4A.2.
- Reference data dalam API Kemnaker masih di-*hardcode* dengan DECODE di kueri (D5 hlm. 3–5), bukan merujuk tabel referensi terpusat.
- Target POC MDM Okt 2026 belum dapat dipastikan tercapai [perlu konfirmasi].

| Aspek | Skor lama → baru | Alasan / sumber |
|---|---|---|
| Identifikasi entitas master | 2 → 3 | Model konseptual 13 entitas berdefinisi (D3-04, D3-05); praktik terdokumentasi sebagian, belum mencakup JKP |
| Kepemilikan & stewardship | 1 → 1 | Data Stewardship hanya ada di diagram (D3-03); keputusan kasus per kasus tanpa SOP (R1 [P 01:06:06–01:08:42]) |
| Golden record & matching | 1 → 2 | POC dengan pembobotan dan pengecekan kualitas (D3-02); belum produksi |
| SoR per entitas | 1 → 2 | Sumber POC teridentifikasi (D3-02); SoR per atribut belum |
| Distribusi | 2 → 2 | Zona penyebaran API masih desain (D3-03); praktik pertukaran memakai DB link (D5) |
| Reference data | 2 → 2 | Kode di-*hardcode* di kueri API (D5 hlm. 3–5) |
| Kualitas & deduplikasi | 2 → 2 | Pengecekan kualitas di POC; metrik capaian belum ada |
| Tools | 1 → 2 | Mesin MDM internal tahap POC (D3-01, D3-02) [perlu konfirmasi] |
| Proses/SOP | 1 → 1 | Belum ada SOP (R1 [P 01:06:06–01:08:42]) |

**Rata-rata baru: ±1,9** (sebelumnya ±1,4). Tambahan: §4A Masukan Desain Golden Record.
