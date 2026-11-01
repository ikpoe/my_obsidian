# 20 — Peta Inisiatif Roadmap (draf kerja)

> Status: draf kerja untuk bab roadmap Blueprint. Lingkup: hanya wilayah yang dicakup ESDP; basis data sumber, SMILE, dan ECM hadir sebagai antarmuka. Tahun 1 diasumsikan dimulai Januari 2027 [perlu konfirmasi]. Blueprint memuat parameter, bukan nilai.

## 1. Pengantar

Sembilan sesi asesmen (berkas 11–19) menghasilkan rata-rata kematangan sekitar ±1,6. Pola yang berulang adalah kebijakan tersedia, tetapi praktik, pemilik, dan bukti belum ada. Karena itu roadmap tidak disusun menurut prioritas abstrak. Urutannya mengikuti ketergantungan: sebuah inisiatif dijalankan bila prasyaratnya sudah tersedia. Setiap inisiatif (RM-n) dapat ditelusuri dari tantangan (sesi, ID aspek, skor) ke prinsip, inisiatif, dan ukuran keberhasilan. Tiga use case TOR dipakai sebagai lensa lintas tahun: UC1 kesiapan infrastruktur ESDP, UC2 API dan pertukaran data, UC3 tata kelola data.

## 2. Tema per tahun

| Tahun | Tema | Alasan |
|---|---|---|
| Tahun 1 | **Fondasi dan Kendali** | Skor terendah ada pada peran, kepemilikan, klasifikasi, retensi, dan ketahanan (TK-2, TK-5, KS-12, PS-4, PS-5 = 1). Tanpa pemilik dan kelas data, integrasi dan pemanfaatan memperbesar risiko. Tahun 1 menetapkan siapa memutuskan, data apa yang dilindungi, dan apakah platform dapat dipulihkan. |
| Tahun 2 | **Integrasi dan Pertukaran Terstandar** | Setelah klasifikasi dan register PKS tersedia, Data Hub dapat dibangun di atas kendali tersebut. Golden record, data contract, dan gold tersertifikasi menyatukan jalur upstream dan downstream melalui ESDP sebagai poros. |
| Tahun 3 | **Pemanfaatan dan Skala** | Pemanfaatan baru aman setelah data tersertifikasi dan aksesnya terkendali. Tahun 3 memperluas self-service, MDM, stewardship, otomasi kebijakan, dan analitik lanjut. |

## 3. Quick win 90 hari

| ID | Quick win | Tantangan | Hasil 90 hari | Owner | Lanjut ke |
|---|---|---|---|---|---|
| QW-1 | Inventaris antarmuka upstream/downstream ESDP | US-4 = 1, PD-9 = 1 | Daftar antarmuka dengan pemilik, pola, frekuensi | Hub/UMD + tim platform | RM-7 |
| QW-2 | Katalog KPI awal (KPI prioritas direksi) | DB-4 = 1 | Definisi, rumus, grain, owner tercatat di Dataedo | Hub/UMD | RM-12 |
| QW-3 | Uji restore klaster (sampel zona gold & metadata) | PS-4 = 1 | Berita acara uji restore, waktu pulih aktual | Custodian (Ops TI) | RM-8 |
| QW-4 | Klarifikasi status DRC untuk platform | PS-5 = 1; konflik D3-01 vs R1 vs D2 hlm. 50 | Keputusan tertulis status DRC | Custodian + KPTI | RM-8, RM-23 |
| QW-5 | Integrasi Kemnaker: tutup kredensial teks biasa, batasi akses DB link | KS-9 = 2 (D5) | Kredensial dirotasi dan keluar dari dokumen; akses dibatasi ke elemen PKS | Custodian + OKI | RM-5, RM-16 |
| QW-6 | Juknis peran (draf) dan penetapan forum | TK-2 = 1, TK-5 = 1 | Draf juknis & RACI dibahas forum/KPTI | Hub/UMD | RM-1 |
| QW-7 | Kebijakan retensi versi awal untuk kelas prioritas | KS-12 = 1, TK-12 = 1 | Draf jadwal retensi siap sebelum 16 Jan 2027 [perlu konfirmasi] | Hub/UMD + domain owner | RM-4 |

## 4. Tabel inisiatif

| ID | Inisiatif | Tema / waktu | Tantangan (sesi, aspek, skor) | Prinsip | Ukuran keberhasilan | Parameter yang ditetapkan | Teknologi eksisting | Owner | Dependensi | UC |
|---|---|---|---|---|---|---|---|---|---|---|
| RM-1 | Juknis peran & forum tata kelola data | Fondasi dan Kendali / Th1 S1 | TK-2 = 1; TK-5 = 1; TK-1 = 2; TK-11 = 1 (s17) | Akuntabilitas sebelum teknologi | Juknis terbit; forum TKD bersidang dan risalah tercatat; pejabat PDP ditunjuk [perlu konfirmasi] | Definisi peran; RACI keputusan; kuorum & frekuensi forum; ambang eskalasi | Dataedo (register peran) | Hub/UMD | — | UC3 |
| RM-2 | Penunjukan owner & steward domain pilot | Fondasi dan Kendali / Th1 S1 | s11 Kepemilikan & stewardship = 1; MK-6 = 1 (s15) | Data dimiliki domain, dikelola hub | % domain pilot dengan owner & steward tercatat di katalog | Daftar domain pilot; kriteria owner/steward; SLA keputusan steward | Dataedo | Hub/UMD + domain owner | RM-1 | UC3 |
| RM-3 | Klasifikasi & label PDP di katalog dan tag Ranger | Fondasi dan Kendali / Th1 S2 | KS-2 = 2; KS-3 = 1 (s16); MK-3 = 2 (s15) | Kontrol mengikuti klasifikasi | % dataset ESDP berklasifikasi & berlabel PDP; % kebijakan Ranger berbasis tag | Kelas klasifikasi; atribut label PDP; aturan pewarisan tag | Dataedo, Atlas, Ranger | Domain owner (A), Hub (R), custodian | RM-2 | UC3, UC1 |
| RM-4 | Kebijakan retensi & minimisasi (versi awal) | Fondasi dan Kendali / Th1 S1 | KS-12 = 1 (s16); TK-12 = 1 (s17); PS-7 = 1 (s18) | Simpan seperlunya, musnahkan dengan bukti | Kebijakan disahkan sebelum PP 33/2026 berlaku [perlu konfirmasi]; jadwal retensi untuk kelas prioritas | Masa retensi per kelas; pemicu awal retensi; metode & bukti pemusnahan; aturan minimisasi elemen | Dataedo (atribut retensi) | Domain owner (A), Hub (R) | RM-1 | UC3 |
| RM-5 | Remediasi integrasi Kemnaker (interim) | Fondasi dan Kendali / Th1 S1 | KS-9 = 2 (s16); DS-3 = 2 (s12); TK-7 = 1 (s17) | Tidak ada nilai rahasia di dokumen; akses minimum | Kredensial dirotasi dan keluar dari dokumen; cakupan akses DB dibatasi ke elemen PKS; adendum PKS memuat retensi & audit | Pemilik secret; periode rotasi; daftar elemen PKS; klausul retensi mitra | 3scale, Ranger | Custodian (Ops TI) + OKI; Unit Pemrakarsa | — | UC2, UC3 |
| RM-6 | Register PKS, templat dokumen teknis & siklus PKS | Fondasi dan Kendali / Th1 S2 | DS-8 = 2 (s12); TK-6 = 2; TK-7 = 1 (s17) | Satu pintu, eligibility dari PKS + dokumen teknis | % PKS aktif di register; % PKS dengan dokumen teknis baku; status adendum Ps. 78 | Atribut register; isi minimum dokumen teknis; periode reviu; syarat pengakhiran | Dataedo (tautan PKS), 3scale | Hub/UMD + Unit Pemrakarsa | RM-1 | UC2, UC3 |
| RM-7 | Inventaris antarmuka & data contract awal | Fondasi dan Kendali / Th1 S1 | US-4 = 1 (s12); PD-9 = 1 (s14); MK-8 = 1 (s15) | Setiap antarmuka punya kontrak dan pemilik | % antarmuka upstream/downstream terinventaris; % prioritas dengan data contract | Atribut inventaris; isi minimum data contract; aturan perubahan skema | Qlik Replicate, Talend, Dataedo | Hub/UMD + tim platform | — | UC2 |
| RM-8 | Ketahanan platform tahap 1 & keputusan DRC | Fondasi dan Kendali / Th1 S1 | PS-4 = 1; PS-5 = 1 (s18); AD-10 = 1 (s13) | Platform harus terbukti dapat dipulihkan | Uji restore klaster berhasil & terdokumentasi; status DRC diputuskan | RPO/RTO per kelas & zona; frekuensi snapshot; frekuensi uji restore; cakupan backup metadata | Cloudera (snapshot HDFS, backup metastore/Atlas/Ranger) | Custodian (Ops TI) | — | UC1 |
| RM-9 | Rencana kapasitas & monitoring platform | Fondasi dan Kendali / Th1 S2 | PS-3 = 2; PS-13 = 2 (s18) | Kapasitas direncanakan, bukan direaksi | Dokumen kapasitas per zona; peringatan ambang aktif | Ambang pemakaian per zona; horizon proyeksi; pemicu penambahan node | Cloudera Manager | Custodian (Ops TI) + tim platform | RM-8 | UC1 |
| RM-10 | Zona medallion & isolasi lingkungan | Fondasi dan Kendali / Th1 S2 | AD-5 = 2 (s13); PS-1 = 2 (s18); KS-11 = 1 (s16) | Zona dengan aturan masuk-keluar | Zona staging/bronze/silver/gold terpisah dengan aturan akses; data non-prod dimasking | Aturan promosi antarzona; konvensi penamaan; aturan data non-prod | Cloudera, Ranger | Tim platform + Hub | RM-3 | UC1 |
| RM-11 | Katalog aktif & lineage | Fondasi dan Kendali / Th1 S2 | MK-3 = 2; MK-4 = 1 (s15); TK-13 = 1 (s17) | Metadata sebagai alat kendali | % dataset dengan owner, klasifikasi, lineage end-to-end | Atribut metadata wajib; cakupan lineage; frekuensi sinkron Atlas–Dataedo | Dataedo, Atlas | Hub/UMD | RM-2 | UC3, UC1 |
| RM-12 | Katalog KPI awal & inventaris laporan | Fondasi dan Kendali / Th1 S1 | DB-4 = 1; DB-6 = 2 (s19) | Satu angka untuk semua pengguna | KPI prioritas terdefinisi dengan owner; inventaris laporan lengkap | Atribut KPI (definisi, rumus, grain, owner, CDE); atribut inventaris laporan | Dataedo, Tableau, QlikSense | Hub/UMD + domain owner | — | UC3 |
| RM-13 | Dimensi Program & Kanal + reference data | Fondasi dan Kendali / Th1 S2 | DB-2 = 2 (s19); PD-6 = 1 (s14); s11 Reference data = 2 | Conformed dimension sebelum KPI | Dimensi Program & Kanal tersedia di gold dan dipakai mart prioritas | Grain dimensi; atribut SCD; pemilik kode referensi | Talend, Cloudera (Kudu/Parquet) | Domain owner + Hub | RM-2 | UC1, UC3 |
| RM-14 | CDE & quality gate per lapis | Fondasi dan Kendali / Th1 S2 | KD-2 = 1; KD-6 = 1 (s15) | Kualitas diukur di titik kritis | % CDE dengan ambang & skor; quality gate aktif di bronze→silver | Daftar CDE; ambang per dimensi; aksi gate gagal | Talend, Dataedo | Domain owner (ambang), steward, Hub | RM-2, RM-11 | UC3 |
| RM-15 | Data Hub (DMZ) & 3scale per PKS | Integrasi dan Pertukaran Terstandar / Th2 S1 | DS-5 = 2; DS-6 = 2 (s12); DB-12 = 1 (s19); PS-9 = 1 (s18) | Satu jalur downstream eksternal | Data Hub produksi; satu application plan per PKS; log per transaksi | Kuota per PKS; freshness produk data; ketersediaan layanan; retensi cache serving | 3scale, Cloudera (gold), Ranger | Hub/UMD (produk) + custodian (operasi) | RM-3, RM-6, RM-10 | UC2, UC1 |
| RM-16 | Migrasi mitra ke Data Hub & penghentian DB link | Integrasi dan Pertukaran Terstandar / Th2 S2 | DS-3 = 2 (s12); KS-9 = 2 (s16) | Tidak ada akses langsung ke DB operasional | Akses DB langsung mitra = nol; seluruh PKS aktif via Data Hub/SFTP | Urutan migrasi mitra; masa transisi; kriteria penutupan akses lama | 3scale | Unit Pemrakarsa + custodian | RM-15, RM-5 | UC2 |
| RM-17 | Pertukaran file terstandar (SFTP/MFT) via Data Hub | Integrasi dan Pertukaran Terstandar / Th2 S1 | US-6 = 2 (s12); PS-11 = 2 (s18) | Validasi otomatis dan masa simpan file | % pertukaran file via jalur baku dengan validasi otomatis & checksum | Masa simpan file masuk/keluar; aturan validasi; format & ukuran file | SFTP eksisting, Talend | Custodian + Hub | RM-4, RM-6 | UC2 |
| RM-18 | Golden record MDM produksi (Peserta & Pemberi Kerja) | Integrasi dan Pertukaran Terstandar / Th2 S1 | s11 Golden record = 2; s11 SoR = 2; s11 Proses/SOP = 1 | Owner domain sebelum golden record | Golden record produksi untuk 2 entitas; antrean steward ber-SLA | Aturan matching & survivorship; SoR per atribut; SLA antrean steward | Mesin MDM internal (POC), Cloudera, Talend | Domain owner + steward; Hub | RM-2, RM-14 | UC1, UC3 |
| RM-19 | Standar ingest, data contract & SLA freshness | Integrasi dan Pertukaran Terstandar / Th2 S1 | US-3 = 2; US-7 = 1 (s12); KD-8 = 1 (s15); PS-6 = 2 (s18) | Freshness sebagai komitmen terukur | % sumber prioritas dengan kontrak & target freshness terpantau | Target freshness per produk; aturan rekonsiliasi; jendela refresh | Qlik Replicate, Talend, Cloudera | Tim platform + owner produk | RM-7 | UC2, UC1 |
| RM-20 | Silver Data Vault & gold tersertifikasi | Integrasi dan Pertukaran Terstandar / Th2 S2 | PD-5 = 2 (s14); DB-1 = 2; DB-11 = 1 (s19) | Historis terintegrasi, penyajian sederhana | Domain inti di Vault; mart prioritas tersertifikasi | Kriteria sertifikasi; standar hub/link/satellite; bus matrix | Cloudera (Kudu/Parquet), Talend | Hub/UMD | RM-13, RM-18 | UC1 |
| RM-21 | Lapisan metrik S2 & akses BI per klasifikasi | Integrasi dan Pertukaran Terstandar / Th2 S2 | DB-3 = 1; DB-10 = 1 (s19) | Satu definisi, akses sesuai klasifikasi | Tableau, QlikSense, Data Hub memakai definisi S2; izin BI terpetakan ke klasifikasi | Pemetaan klasifikasi → peran BI; aturan row-level; aturan ekspor | Dataedo, Tableau, QlikSense, Ranger | Hub/UMD + OKI | RM-12, RM-20, RM-3 | UC3, UC1 |
| RM-22 | Tiering & retensi otomatis di platform | Integrasi dan Pertukaran Terstandar / Th2 S2 | PS-7 = 1; PS-8 = 1 (s18) | Retensi sebelum tiering | % kelas data dengan tiering aktif; bukti pemusnahan tercatat | Batas panas/hangat/dingin; umur staging; versi Parquet disimpan | Cloudera (HDFS storage policy), Atlas | Custodian (eksekusi), Hub | RM-4, RM-11 | UC1, UC3 |
| RM-23 | Klaster DRC warm-standby (gold & serving) | Integrasi dan Pertukaran Terstandar / Th2 S2 | PS-5 = 1 (s18); AD-10 = 1 (s13) | Ketahanan bertingkat per kelas data | Replikasi gold/serving aktif; uji failover pertama berhasil | RPO/RTO kelas kritikal; frekuensi replikasi; kapasitas DRC | Cloudera (replication) | Custodian (Ops TI) | RM-8, RM-15 | UC1 |
| RM-24 | Manajemen isu data & metrik tata kelola | Integrasi dan Pertukaran Terstandar / Th2 S1 | TK-9 = 2; TK-10 = 1 (s17); KD-5 = 2 (s15) | Isu data punya pemilik dan SLA | % isu selesai dalam SLA; laporan metrik tata kelola ke KPTI | Kategori & prioritas isu; SLA; ambang eskalasi; daftar metrik | ITSM eksisting [perlu konfirmasi], Dataedo | Hub/UMD + steward | RM-1, RM-14 | UC3 |
| RM-25 | Literasi data & kualitas di sumber | Integrasi dan Pertukaran Terstandar / Th2 S2 | KD-9 = 1 (s15); TK-14 = 1 (s17) | Kualitas dibangun di titik input | Program pelatihan berjalan; tren isu dari cabang menurun | Kelompok sasaran; kurikulum; indikator unit | Dashboard Kualitas Data (Tableau/QlikSense) | Hub/UMD + Kanwil | RM-24 | UC3 |
| RM-26 | Governed self-service BI & rasionalisasi laporan | Pemanfaatan dan Skala / Th3 S1 | DB-8 = 1; DB-6 = 2; DB-5 = 2 (s19) | Pengguna membangun di atas data tersertifikasi | % dashboard dari dataset tersertifikasi; laporan duplikat dipensiunkan | Kelompok pengguna; kuota; syarat publikasi; ambang pemakaian minimum | Tableau, QlikSense | Hub/UMD + domain owner | RM-21 | UC3, UC1 |
| RM-27 | Delegasi stewardship ke semua domain | Pemanfaatan dan Skala / Th3 S1 | TK-2 = 1; TK-3 = 2 (s17) | Federasi bertahap | Semua domain DBB punya owner & steward aktif | Cakupan domain; kriteria delegasi | Dataedo | Hub/UMD + domain owner | RM-24 | UC3 |
| RM-28 | Perluasan MDM & distribusi ke aplikasi | Pemanfaatan dan Skala / Th3 S1 | s11 Distribusi = 2; DS-2 = 1 (s12) | Golden record dipakai, bukan disimpan | Aplikasi prioritas mengonsumsi golden record via API | Entitas tambahan; kontrak distribusi; SLA sinkronisasi | Mesin MDM, 3scale | Hub/UMD + domain owner | RM-18, RM-15 | UC2, UC1 |
| RM-29 | Policy-as-code & audit akses otomatis | Pemanfaatan dan Skala / Th3 S1 | KS-8 = 1; KS-5 = 2 (s16) | Kebijakan ditegakkan mesin | Audit akses data Rahasia otomatis; reviu akses berkala terpenuhi | Peristiwa audit wajib; periode reviu akses; aturan alert | Ranger, Atlas | OKI + custodian | RM-11, RM-3 | UC3, UC1 |
| RM-30 | Analitik lanjut di atas gold | Pemanfaatan dan Skala / Th3 S2 | DB-13 = 2 (s19) | Nilai bisnis dari data terpercaya | Model forecasting & risiko likuiditas di produksi dengan monitoring | Kriteria deployment model; metrik monitoring model | Python/KNIME, Cloudera | Hub/UMD + unit bisnis | RM-20 | UC1 |
| RM-31 | Uji failover rutin & SLA layanan Data Hub | Pemanfaatan dan Skala / Th3 S2 | DS-7 = 1 (s12); PS-5 = 1 (s18) | Komitmen layanan terukur | Uji failover berkala lulus; laporan SLA ke mitra | Frekuensi uji; target ketersediaan; isi laporan SLA | Cloudera, 3scale | Custodian | RM-23 | UC1, UC2 |
| RM-32 | Evaluasi kematangan & revisi Perdir | Pemanfaatan dan Skala / Th3 S2 | AD-11 = 2 (s13); TK-4 = 2 (s17) | Kebijakan mengikuti praktik | Asesmen ulang lintas sesi; usulan revisi D1 (peran, retensi, minimisasi) | Metode asesmen; periode reviu kebijakan | — | Hub/UMD + KPTI | RM-27 | UC3 |

Catatan: "s11" sampai "s19" merujuk berkas sesi 11–19. Berkas 11 tidak memakai ID aspek, sehingga aspeknya dirujuk dengan nama.

## 5. Daftar dependensi

Aturan urutan utama:
- Owner domain (RM-2) mendahului golden record (RM-18).
- Klasifikasi (RM-3) mendahului Data Hub (RM-15).
- Dimensi Program/Kanal (RM-13) mendahului KPI konsisten di lapisan metrik (RM-21).
- Kebijakan retensi (RM-4) mendahului tiering (RM-22).
- Keputusan DRC (RM-8) mendahului DRC warm-standby (RM-23).

| Inisiatif | Bergantung pada |
|---|---|
| RM-2 Penunjukan owner & steward domain pilot | RM-1 Juknis peran & forum tata kelola data |
| RM-3 Klasifikasi & label PDP di katalog dan tag Ranger | RM-2 Penunjukan owner & steward domain pilot |
| RM-4 Kebijakan retensi & minimisasi (versi awal) | RM-1 Juknis peran & forum tata kelola data |
| RM-6 Register PKS, templat dokumen teknis & siklus PKS | RM-1 Juknis peran & forum tata kelola data |
| RM-9 Rencana kapasitas & monitoring platform | RM-8 Ketahanan platform tahap 1 & keputusan DRC |
| RM-10 Zona medallion & isolasi lingkungan | RM-3 Klasifikasi & label PDP di katalog dan tag Ranger |
| RM-11 Katalog aktif & lineage | RM-2 Penunjukan owner & steward domain pilot |
| RM-13 Dimensi Program & Kanal + reference data | RM-2 Penunjukan owner & steward domain pilot |
| RM-14 CDE & quality gate per lapis | RM-2 Penunjukan owner & steward domain pilot, RM-11 Katalog aktif & lineage |
| RM-15 Data Hub (DMZ) & 3scale per PKS | RM-3 Klasifikasi & label PDP di katalog dan tag Ranger, RM-6 Register PKS, templat dokumen teknis & siklus PKS, RM-10 Zona medallion & isolasi lingkungan |
| RM-16 Migrasi mitra ke Data Hub & penghentian DB link | RM-15 Data Hub (DMZ) & 3scale per PKS, RM-5 Remediasi integrasi Kemnaker (interim) |
| RM-17 Pertukaran file terstandar (SFTP/MFT) via Data Hub | RM-4 Kebijakan retensi & minimisasi (versi awal), RM-6 Register PKS, templat dokumen teknis & siklus PKS |
| RM-18 Golden record MDM produksi (Peserta & Pemberi Kerja) | RM-2 Penunjukan owner & steward domain pilot, RM-14 CDE & quality gate per lapis |
| RM-19 Standar ingest, data contract & SLA freshness | RM-7 Inventaris antarmuka & data contract awal |
| RM-20 Silver Data Vault & gold tersertifikasi | RM-13 Dimensi Program & Kanal + reference data, RM-18 Golden record MDM produksi (Peserta & Pemberi Kerja) |
| RM-21 Lapisan metrik S2 & akses BI per klasifikasi | RM-12 Katalog KPI awal & inventaris laporan, RM-20 Silver Data Vault & gold tersertifikasi, RM-3 Klasifikasi & label PDP di katalog dan tag Ranger |
| RM-22 Tiering & retensi otomatis di platform | RM-4 Kebijakan retensi & minimisasi (versi awal), RM-11 Katalog aktif & lineage |
| RM-23 Klaster DRC warm-standby (gold & serving) | RM-8 Ketahanan platform tahap 1 & keputusan DRC, RM-15 Data Hub (DMZ) & 3scale per PKS |
| RM-24 Manajemen isu data & metrik tata kelola | RM-1 Juknis peran & forum tata kelola data, RM-14 CDE & quality gate per lapis |
| RM-25 Literasi data & kualitas di sumber | RM-24 Manajemen isu data & metrik tata kelola |
| RM-26 Governed self-service BI & rasionalisasi laporan | RM-21 Lapisan metrik S2 & akses BI per klasifikasi |
| RM-27 Delegasi stewardship ke semua domain | RM-24 Manajemen isu data & metrik tata kelola |
| RM-28 Perluasan MDM & distribusi ke aplikasi | RM-18 Golden record MDM produksi (Peserta & Pemberi Kerja), RM-15 Data Hub (DMZ) & 3scale per PKS |
| RM-29 Policy-as-code & audit akses otomatis | RM-11 Katalog aktif & lineage, RM-3 Klasifikasi & label PDP di katalog dan tag Ranger |
| RM-30 Analitik lanjut di atas gold | RM-20 Silver Data Vault & gold tersertifikasi |
| RM-31 Uji failover rutin & SLA layanan Data Hub | RM-23 Klaster DRC warm-standby (gold & serving) |
| RM-32 Evaluasi kematangan & revisi Perdir | RM-27 Delegasi stewardship ke semua domain |

**Jalur kritis (rantai ketergantungan terpanjang, 8 langkah):** RM-1 → RM-2 → RM-11 → RM-14 → RM-18 → RM-20 → RM-21 → RM-26. Keterlambatan juknis peran (RM-1) menggeser golden record, gold tersertifikasi, lapisan metrik, dan self-service. Jalur kedua: RM-1 → RM-2 → RM-3 → RM-10 → RM-15 → RM-23 → RM-31.

## 6. Tonggak eksternal

| Tonggak | Waktu | Inisiatif terdampak | Catatan konflik |
|---|---|---|---|
| PP 33/2026 berlaku (retensi Ps. 75, minimisasi Ps. 59) | 16 Jan 2027 [perlu konfirmasi] | RM-4, RM-22, RM-5, RM-17 | **Konflik.** Sisa waktu sekitar 2,5 bulan dari 1 Nov 2026, sebelum Tahun 1 dimulai. Owner retensi (TK-12 = 1) belum ada. Kebijakan awal harus ditarik ke QW-7 dengan kepemilikan sementara di Hub/UMD. Otomasi (RM-22) baru Tahun 2. |
| Target POC MDM | Okt 2026 | RM-18, RM-28 | **Konflik.** Target sudah lewat; statusnya belum terkonfirmasi (11-Klarifikasi). Bila manajemen mendorong produksi segera, aturan "owner domain sebelum golden record" dilanggar. Usulan: POC dibekukan sebagai baseline, produksi Tahun 2 S1. |
| Data Hub (bangun baru, DMZ) | Jadwal BPJS [perlu konfirmasi] | RM-15, RM-16, RM-28 | **Konflik potensial.** Bila pembangunan berjalan sebelum klasifikasi (RM-3) dan register PKS (RM-6), kontrol akses berbasis kelas tidak dapat ditegakkan. Usulan: klasifikasi minimum untuk produk data pilot sebagai gerbang go-live. |
| Status DRC | Belum terselesaikan | RM-8, RM-23, RM-31, RM-15 | **Konflik.** D3-01, R1, dan D2 hlm. 50 bertentangan. Data Hub (Tahun 2 S1) akan berjalan tanpa DR platform sampai RM-23 (Tahun 2 S2). |
| Penutupan DB link Kemnaker | QW (interim) vs Tahun 2 S2 (final) | RM-5, RM-16 | Penutupan penuh menunggu Data Hub. Selama masa itu berlaku kontrol interim (QW-5). |
| Integrasi Taspen/Asabri | 2029 (s11, tidak dinilai) | RM-28 | Di luar horizon; dipantau sebagai masukan perluasan MDM. |

## 7. Matriks cakupan use case TOR

| Use case | Tahun 1 | Tahun 2 | Tahun 3 | Jumlah |
|---|---|---|---|---|
| UC1 Kesiapan infrastruktur ESDP | RM-3, RM-8, RM-9, RM-10, RM-11, RM-13 | RM-15, RM-18, RM-19, RM-20, RM-21, RM-22, RM-23 | RM-26, RM-28, RM-29, RM-30, RM-31 | 18 |
| UC2 API & pertukaran data | RM-5, RM-6, RM-7 | RM-15, RM-16, RM-17, RM-19 | RM-28, RM-31 | 9 |
| UC3 Tata kelola data | RM-1, RM-2, RM-3, RM-4, RM-5, RM-6, RM-11, RM-12, RM-13, RM-14 | RM-18, RM-21, RM-22, RM-24, RM-25 | RM-26, RM-27, RM-29, RM-32 | 19 |

Celah cakupan:
- UC2 tipis di Tahun 1. Isinya hanya persiapan (RM-5, RM-6, RM-7), karena Data Hub sengaja ditunda sampai klasifikasi siap.
- ODS/operational serving (AD-6 = 1) belum memiliki inisiatif. Kebutuhannya bergantung pada konfirmasi pola konsumsi operasional.
- Kapasitas jangka panjang terkait volume peserta hanya dicakup RM-9 di tingkat rencana.

## 8. Pertanyaan terbuka yang menghambat pengurutan

1. Kapan Tahun 1 dimulai (Jan 2027 atau mengikuti tahun anggaran/RKA)? [perlu konfirmasi]
2. Apakah PP 33/2026 berlaku 16 Jan 2027, dan adakah masa transisi untuk retensi dan minimisasi? [perlu konfirmasi]
3. Apa status POC MDM terhadap target Okt 2026, dan apa keputusan lanjutannya? (11-Klarifikasi)
4. Apa status DRC untuk platform: aktif, sebagian, atau belum ada? (18-Klarifikasi)
5. Bagaimana jadwal, anggaran, dan pemilik pembangunan Data Hub, termasuk apakah go-live dapat digerbangi klasifikasi? [perlu konfirmasi]
6. Apa mandat *data governance council* yang sudah ada, dan apakah ia menjadi forum di RM-1? (17-Klarifikasi 4a)
7. Apakah pejabat PDP (UU 27/2022 Ps. 53) sudah ditunjuk? [perlu konfirmasi]
8. Domain mana yang menjadi pilot 2–3 domain Tahun 1?
9. Apakah ada kebutuhan ODS untuk konsumsi operasional? (AD-6)

## Sumber

Berkas 11–19 beserta Klarifikasi; 00-Analisis-Dokumen-BPJS.md; D1, D2 hlm. 22 dan 50, D3-01, D3-02, D4, D5 (temuan saja); R1; PP 33/2026.
