---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Master & Reference Data
sesi: 1
blueprint: [3.4, 5.2, 5.3]
status: draft-diskusi
acuan_internal: DAMA-DMBOK v2, bab Reference & Master Data
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, analisis_dokumen_bpjs.md, bpjstk_tender_inventory.csv]
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

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| Identifikasi entitas master (peserta/TK, PK/BU, wilayah, kantor cabang, program, kode referensi) | Daftar entitas master & referensi yang disepakati, lengkap dengan definisi dan atribut kunci | Kebutuhan & lingkup MD&DR memuat entitas & elemen data, pemetaan ke sistem berjalan, dan analisis variansi, disusun bertahap sesuai prioritas (D1 hlm. 87–88) | Data Building Block mencakup Data Tenaga Kerja, Data Primer/Sekunder Peserta, Data Pemberi Kerja/Badan Usaha, Data Profil Kantor Daerah, Data Mitra Perbankan (D2 hlm. 16). Belum ada daftar resmi entitas master | Belum ada katalog entitas master yang ditetapkan. Wilayah, program, dan kode referensi tidak terlihat sebagai master/referensi | 2 |
| Kepemilikan & stewardship | Owner bisnis & steward per entitas, RACI | Prinsip "penetapan kepemilikan" MD&DR (D1 hlm. 87). Unit Manajemen Data bertugas atas master data & data referensi (D1 hlm. 9, Ps. 8(3)). Kancab sebagai *data producer* (D1 hlm. 10). Peran *data liaison* (D1 hlm. 89) | Fungsi pengelola master data belum didefinisikan secara jelas (D2 hlm. 24). Kewenangan pengubahan elemen data tidak jelas (D2 hlm. 26). Data Steward baru muncul di ilustrasi target (D2 hlm. 23) | Owner/steward per entitas belum ditetapkan | 1 |
| Golden record & aturan matching/merge (NIK sebagai kunci, Dukcapil) | Aturan identifikasi, *match/merge/survivorship*, dan verifikasi ke sumber otoritatif | Fungsi "pengelolaan identitas data" dan konsolidasi ke MD&DR (D1 hlm. 88). Contoh aturan validitas: NIK 16 digit numerik (D1 hlm. 84) | Tidak ada bukti aturan match/merge atau golden record. Integrasi Dukcapil tidak disebut di D2 [perlu konfirmasi] | Aturan matching, *survivorship*, dan penanganan NIK ganda/kosong belum ada | 1 |
| Sumber kebenaran / SoR per entitas | SoR ditetapkan per entitas/atribut | Single master data repository terpusat (D1 hlm. 88) | SMILE adalah core system yang dipakai di seluruh direktorat (D2 hlm. 35). Ada aplikasi yang belum terintegrasi dengan SMILE (D2 hlm. 45). Big data belum tentu SSoT (D2 hlm. 24) | SoR per entitas tidak terdokumentasi. Asumsi "SMILE = SoR Peserta/PK" [perlu konfirmasi] | 1 |
| Distribusi/sinkronisasi ke konsumen | Layanan *publish/subscribe* atau API master data dengan SLA | Fungsi "sharing" MD&DR (D1 hlm. 88). Sistem wajib menjaga konsistensi nilai master data (D1 hlm. 88). Publikasi melalui data liaison (D1 hlm. 89) | Integrasi via ADG, Talend, Qlik Replicate, API (D2 hlm. 21). Mekanisme ini bersifat replikasi data, bukan distribusi master data. Big data tidak mengacu master data (D2 hlm. 24) | Belum ada layanan distribusi master data | 2 |
| Reference data / kode standar | Kode referensi dikelola terpusat dengan *versioning* dan pemetaan ke standar eksternal | MD&DR dikelola untuk dibagipakaikan dan dievaluasi minimal setahun sekali (D1 hlm. 30, Ps. 32). Kamus data memuat "Example of Valid Values" (D2 hlm. 25, sebagai standar) | Perbedaan nama & definisi elemen data antar-skema di Kamus Data (D2 hlm. 26). Pengelolaan kode referensi (wilayah, program, dll.) tidak dibahas [perlu konfirmasi] | Belum ada daftar kode referensi resmi dan pemiliknya | 2 |
| Kualitas & deduplikasi | Metrik keunikan/konsistensi master dipantau, deduplikasi berkelanjutan | Dimensi uniqueness & consistency, termasuk metrik konsistensi terhadap SMILE (D1 hlm. 84). MD&DR dipantau berkala (D1 hlm. 87) | Masih ditemukan duplikasi, inkonsistensi, dan format tidak valid. Perbaikan bad data di kancab belum optimal (D2 hlm. 28). Ada proses DQ as-is: profiling, cleansing (D2 hlm. 27) | DQ berjalan, tetapi tidak terkait golden record. Tingkat duplikasi tidak terukur di dokumen [perlu konfirmasi] | 2 |
| Tools | Tool MDM (hub/registry) dan tool reference data | Arsitektur MD&DR: identitas, create/update, sharing (D1 hlm. 88). Tidak menyebut produk | Manual, tanpa tool MDM (D2 hlm. 24). Ilustrasi target berupa MDM tools (D2 hlm. 23). Tidak ada tender MDM di inventaris (Cloudera, Dataedo, Qlik saja) | Tidak ada tool | 1 |
| Proses/SOP | SOP create/update/retire, alur persetujuan perubahan, review berkala | Perubahan terkendali (D1 hlm. 87). Review minimal 1x/tahun (D1 hlm. 89; Ps. 32(3) hlm. 30) | Pengelolaan & penggunaan master data tidak diatur dalam SOP, sehingga evaluasi tidak dapat dilakukan (D2 hlm. 24) | SOP belum ada | 1 |
| Implikasi integrasi Taspen/Asabri 2029 | Identitas peserta lintas lembaga dapat dicocokkan, kode referensi selaras | Tidak disebut di D1. D1 hanya mengatur Kolaborasi & Pemanfaatan Data secara umum (D1 hlm. 9, definisi) | Tidak disebut di D2 | Kebutuhan master data lintas lembaga belum terpetakan. Dasar dan jadwal 2029 [perlu konfirmasi] | – (tidak dinilai, belum ada bukti) |

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
