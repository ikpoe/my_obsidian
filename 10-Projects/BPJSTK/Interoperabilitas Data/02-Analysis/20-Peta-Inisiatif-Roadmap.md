# 20 — Peta Inisiatif Roadmap (draf kerja)

> Status: draf kerja untuk bab roadmap Blueprint. Lingkup: hanya wilayah yang dicakup ESDP; basis data sumber, SMILE, dan ECM hadir sebagai antarmuka. Jadwal disusun per kuartal (Y1Q1–Y3Q4). Dengan asumsi Tahun 1 dimulai Januari 2027 [perlu konfirmasi], Y1Q1–Y1Q4 setara Jan–Mar sampai Okt–Des 2027, Y2 setara 2028, dan Y3 setara 2029. Kuartal dipilih agar selaras dengan RKAT dan laporan triwulanan [perlu konfirmasi siklus pelaporan internal]. Blueprint memuat parameter, bukan nilai.

## 1. Pengantar

Sembilan sesi asesmen (berkas 11–19) menghasilkan rata-rata kematangan sekitar ±1,6. Pola yang berulang adalah kebijakan tersedia, tetapi praktik, pemilik, dan bukti belum ada. Karena itu roadmap tidak disusun menurut prioritas abstrak. Urutannya mengikuti ketergantungan: sebuah inisiatif dijalankan bila prasyaratnya sudah tersedia. Setiap inisiatif (RM-n) dapat ditelusuri dari tantangan (sesi, ID aspek, skor) ke prinsip, inisiatif, dan ukuran keberhasilan. Tiga use case TOR dipakai sebagai lensa lintas tahun: UC1 kesiapan infrastruktur ESDP, UC2 API dan pertukaran data, UC3 tata kelola data.

## 2. Tema per tahun

| Tahun | Tema | Alasan |
|---|---|---|
| Tahun 1 | **Fondasi dan Kendali** | Skor terendah ada pada peran, kepemilikan, klasifikasi, retensi, dan ketahanan (TK-2, TK-5, KS-12, PS-4, PS-5 = 1). Tanpa pemilik dan kelas data, integrasi dan pemanfaatan memperbesar risiko. Tahun 1 menetapkan siapa memutuskan, data apa yang dilindungi, dan apakah platform dapat dipulihkan. |
| Tahun 2 | **Integrasi dan Pertukaran Terstandar** | Setelah klasifikasi dan register PKS tersedia, Data Hub dapat dibangun di atas kendali tersebut. Golden record, data contract, dan gold tersertifikasi menyatukan jalur upstream dan downstream melalui ESDP sebagai poros. |
| Tahun 3 | **Pemanfaatan dan Skala** | Pemanfaatan baru aman setelah data tersertifikasi dan aksesnya terkendali. Tahun 3 memperluas self-service, MDM, stewardship, otomasi kebijakan, dan analitik lanjut. |

## 3. Tabel inisiatif

| ID | Inisiatif | Tema / mulai–selesai | Tantangan (sesi, aspek, skor) | Prinsip | Ukuran keberhasilan | Parameter yang ditetapkan | Teknologi eksisting | Owner | Dependensi | UC |
|---|---|---|---|---|---|---|---|---|---|---|
| RM-1 | Juknis peran & forum tata kelola data | Fondasi dan Kendali / Y1Q1–Y1Q2 | TK-2 = 1; TK-5 = 1; TK-1 = 2; TK-11 = 1 (s17) | Akuntabilitas sebelum teknologi | Draf juknis & RACI dibahas forum/KPTI; juknis terbit; forum TKD bersidang dan risalah tercatat; pejabat PDP ditunjuk [perlu konfirmasi] | Definisi peran; RACI keputusan; kuorum & frekuensi forum; ambang eskalasi | Dataedo (register peran) | Hub/UMD | — | UC3 |
| RM-2 | Penunjukan owner & steward domain pilot | Fondasi dan Kendali / Y1Q2–Y1Q2 | s11 Kepemilikan & stewardship = 1; MK-6 = 1 (s15) | Data dimiliki domain, dikelola hub | % domain pilot dengan owner & steward tercatat di katalog | Daftar domain pilot; kriteria owner/steward; SLA keputusan steward | Dataedo | Hub/UMD + domain owner | RM-1 | UC3 |
| RM-3 | Klasifikasi & label PDP di katalog dan tag Ranger | Fondasi dan Kendali / Y1Q3–Y1Q3 | KS-2 = 2; KS-3 = 1 (s16); MK-3 = 2 (s15) | Kontrol mengikuti klasifikasi | % dataset ESDP berklasifikasi & berlabel PDP; % kebijakan Ranger berbasis tag | Kelas klasifikasi; atribut label PDP; aturan pewarisan tag | Dataedo, Atlas, Ranger | Domain owner (A), Hub (R), custodian | RM-2 | UC3, UC1 |
| RM-4 | Kebijakan retensi & minimisasi (versi awal) | Fondasi dan Kendali / Y1Q1–Y1Q2 | KS-12 = 1 (s16); TK-12 = 1 (s17); PS-7 = 1 (s18) | Simpan seperlunya, musnahkan dengan bukti | Jadwal retensi awal kelas prioritas siap sebelum 16 Jan 2027; kebijakan disahkan sebelum PP 33/2026 berlaku [perlu konfirmasi]; jadwal retensi untuk kelas prioritas | Masa retensi per kelas; pemicu awal retensi; metode & bukti pemusnahan; aturan minimisasi elemen | Dataedo (atribut retensi) | Domain owner (A), Hub (R) | RM-1 | UC3 |
| RM-5 | Remediasi integrasi Kemnaker (interim) | Fondasi dan Kendali / Y1Q1–Y1Q2 | KS-9 = 2 (s16); DS-3 = 2 (s12); TK-7 = 1 (s17) | Tidak ada nilai rahasia di dokumen; akses minimum | Langkah awal: kredensial teks biasa dirotasi dan keluar dari dokumen; akses DB link dibatasi ke elemen PKS; adendum PKS memuat retensi & audit | Pemilik secret; periode rotasi; daftar elemen PKS; klausul retensi mitra | 3scale, Ranger | Custodian (Ops TI) + OKI; Unit Pemrakarsa | — | UC2, UC3 |
| RM-6 | Register PKS, templat dokumen teknis & siklus PKS | Fondasi dan Kendali / Y1Q2–Y1Q3 | DS-8 = 2 (s12); TK-6 = 2; TK-7 = 1 (s17) | Satu pintu, eligibility dari PKS + dokumen teknis | % PKS aktif di register; % PKS dengan dokumen teknis baku; status adendum Ps. 78 | Atribut register; isi minimum dokumen teknis; periode reviu; syarat pengakhiran | Dataedo (tautan PKS), 3scale | Hub/UMD + Unit Pemrakarsa | RM-1 | UC2, UC3 |
| RM-7 | Inventaris antarmuka & data contract awal | Fondasi dan Kendali / Y1Q1–Y1Q2 | US-4 = 1 (s12); PD-9 = 1 (s14); MK-8 = 1 (s15) | Setiap antarmuka punya kontrak dan pemilik | Daftar antarmuka (pemilik, pola, frekuensi) lengkap; % antarmuka upstream/downstream terinventaris; % prioritas dengan data contract | Atribut inventaris; isi minimum data contract; aturan perubahan skema | Qlik Replicate, Talend, Dataedo | Hub/UMD + tim platform | — | UC2 |
| RM-8 | Ketahanan platform tahap 1 & keputusan DRC | Fondasi dan Kendali / Y1Q1–Y1Q3 | PS-4 = 1; PS-5 = 1 (s18), status DRC bertentangan; AD-10 = 1 (s13) | Platform harus terbukti dapat dipulihkan | Langkah awal: keputusan tertulis status DRC (konflik D3-01, R1, D2 hlm. 50) dan uji restore sampel gold & metadata; berita acara uji restore; uji restore berkala berjalan | RPO/RTO per kelas & zona; frekuensi snapshot; frekuensi uji restore; cakupan backup metadata | Cloudera (snapshot HDFS, backup metastore/Atlas/Ranger) | Custodian (Ops TI) | — | UC1 |
| RM-9 | Rencana kapasitas & monitoring platform | Fondasi dan Kendali / Y1Q4–Y1Q4 | PS-3 = 2; PS-13 = 2 (s18) | Kapasitas direncanakan, bukan direaksi | Dokumen kapasitas per zona; peringatan ambang aktif | Ambang pemakaian per zona; horizon proyeksi; pemicu penambahan node | Cloudera Manager | Custodian (Ops TI) + tim platform | RM-8 | UC1 |
| RM-10 | Zona medallion & isolasi lingkungan | Fondasi dan Kendali / Y1Q4–Y1Q4 | AD-5 = 2 (s13); PS-1 = 2 (s18); KS-11 = 1 (s16) | Zona dengan aturan masuk-keluar | Zona staging/bronze/silver/gold terpisah dengan aturan akses; data non-prod dimasking | Aturan promosi antarzona; konvensi penamaan; aturan data non-prod | Cloudera, Ranger | Tim platform + Hub | RM-3 | UC1 |
| RM-11 | Katalog aktif & lineage | Fondasi dan Kendali / Y1Q3–Y1Q4 | MK-3 = 2; MK-4 = 1 (s15); TK-13 = 1 (s17) | Metadata sebagai alat kendali | % dataset dengan owner, klasifikasi, lineage end-to-end | Atribut metadata wajib; cakupan lineage; frekuensi sinkron Atlas–Dataedo | Dataedo, Atlas | Hub/UMD | RM-2 | UC3, UC1 |
| RM-12 | Katalog KPI awal & inventaris laporan | Fondasi dan Kendali / Y1Q1–Y1Q2 | DB-4 = 1; DB-6 = 2 (s19) | Satu angka untuk semua pengguna | KPI prioritas direksi terdefinisi (definisi, rumus, grain, owner) di Dataedo; inventaris laporan lengkap | Atribut KPI (definisi, rumus, grain, owner, CDE); atribut inventaris laporan | Dataedo, Tableau, QlikSense | Hub/UMD + domain owner | — | UC3 |
| RM-13 | Dimensi Program & Kanal + reference data | Fondasi dan Kendali / Y1Q4–Y2Q1 | DB-2 = 2 (s19); PD-6 = 1 (s14); s11 Reference data = 2 | Conformed dimension sebelum KPI | Dimensi Program & Kanal tersedia di gold dan dipakai mart prioritas | Grain dimensi; atribut SCD; pemilik kode referensi | Talend, Cloudera (Kudu/Parquet) | Domain owner + Hub | RM-2 | UC1, UC3 |
| RM-14 | CDE & quality gate per lapis | Fondasi dan Kendali / Y1Q4–Y1Q4 | KD-2 = 1; KD-6 = 1 (s15) | Kualitas diukur di titik kritis | % CDE dengan ambang & skor; quality gate aktif di bronze→silver | Daftar CDE; ambang per dimensi; aksi gate gagal | Talend, Dataedo | Domain owner (ambang), steward, Hub | RM-2, RM-11 | UC3 |
| RM-15 | Data Hub (DMZ) & 3scale per PKS | Integrasi dan Pertukaran Terstandar / Y2Q1–Y2Q2 | DS-5 = 2; DS-6 = 2 (s12); DB-12 = 1 (s19); PS-9 = 1 (s18) | Satu jalur downstream eksternal | Data Hub produksi; satu application plan per PKS; log per transaksi | Kuota per PKS; freshness produk data; ketersediaan layanan; retensi cache serving | 3scale, Cloudera (gold), Ranger | Hub/UMD (produk) + custodian (operasi) | RM-3, RM-6, RM-10 | UC2, UC1 |
| RM-16 | Migrasi mitra ke Data Hub & penghentian DB link | Integrasi dan Pertukaran Terstandar / Y2Q3–Y2Q4 | DS-3 = 2 (s12); KS-9 = 2 (s16) | Tidak ada akses langsung ke DB operasional | Akses DB langsung mitra = nol; seluruh PKS aktif via Data Hub/SFTP | Urutan migrasi mitra; masa transisi; kriteria penutupan akses lama | 3scale | Unit Pemrakarsa + custodian | RM-15, RM-5 | UC2 |
| RM-17 | Pertukaran file terstandar (SFTP/MFT) via Data Hub | Integrasi dan Pertukaran Terstandar / Y2Q1–Y2Q2 | US-6 = 2 (s12); PS-11 = 2 (s18) | Validasi otomatis dan masa simpan file | % pertukaran file via jalur baku dengan validasi otomatis & checksum | Masa simpan file masuk/keluar; aturan validasi; format & ukuran file | SFTP eksisting, Talend | Custodian + Hub | RM-4, RM-6 | UC2 |
| RM-18 | Golden record MDM produksi (Peserta & Pemberi Kerja) | Integrasi dan Pertukaran Terstandar / Y2Q1–Y2Q2 | s11 Golden record = 2; s11 SoR = 2; s11 Proses/SOP = 1 | Owner domain sebelum golden record | Golden record produksi untuk 2 entitas; antrean steward ber-SLA | Aturan matching & survivorship; SoR per atribut; SLA antrean steward | Mesin MDM internal (POC), Cloudera, Talend | Domain owner + steward; Hub | RM-2, RM-14 | UC1, UC3 |
| RM-19 | Standar ingest, data contract & SLA freshness | Integrasi dan Pertukaran Terstandar / Y2Q1–Y2Q2 | US-3 = 2; US-7 = 1 (s12); KD-8 = 1 (s15); PS-6 = 2 (s18) | Freshness sebagai komitmen terukur | % sumber prioritas dengan kontrak & target freshness terpantau | Target freshness per produk; aturan rekonsiliasi; jendela refresh | Qlik Replicate, Talend, Cloudera | Tim platform + owner produk | RM-7 | UC2, UC1 |
| RM-20 | Silver Data Vault & gold tersertifikasi | Integrasi dan Pertukaran Terstandar / Y2Q3–Y2Q4 | PD-5 = 2 (s14); DB-1 = 2; DB-11 = 1 (s19) | Historis terintegrasi, penyajian sederhana | Domain inti di Vault; mart prioritas tersertifikasi | Kriteria sertifikasi; standar hub/link/satellite; bus matrix | Cloudera (Kudu/Parquet), Talend | Hub/UMD | RM-13, RM-18 | UC1 |
| RM-21 | Lapisan metrik bersama & akses BI per klasifikasi | Integrasi dan Pertukaran Terstandar / Y2Q4–Y3Q1 | DB-3 = 1; DB-10 = 1 (s19) | Satu definisi, akses sesuai klasifikasi | Tableau, QlikSense, Data Hub memakai definisi metrik bersama; izin BI terpetakan ke klasifikasi | Pemetaan klasifikasi → peran BI; aturan row-level; aturan ekspor | Dataedo, Tableau, QlikSense, Ranger | Hub/UMD + OKI | RM-12, RM-20, RM-3 | UC3, UC1 |
| RM-22 | Tiering & retensi otomatis di platform | Integrasi dan Pertukaran Terstandar / Y2Q3–Y2Q4 | PS-7 = 1; PS-8 = 1 (s18) | Retensi sebelum tiering | % kelas data dengan tiering aktif; bukti pemusnahan tercatat | Batas panas/hangat/dingin; umur staging; versi Parquet disimpan | Cloudera (HDFS storage policy), Atlas | Custodian (eksekusi), Hub | RM-4, RM-11 | UC1, UC3 |
| RM-23 | Klaster DRC warm-standby (gold & serving) | Integrasi dan Pertukaran Terstandar / Y2Q3–Y2Q4 | PS-5 = 1 (s18); AD-10 = 1 (s13) | Ketahanan bertingkat per kelas data | Replikasi gold/serving aktif; uji failover pertama berhasil | RPO/RTO kelas kritikal; frekuensi replikasi; kapasitas DRC | Cloudera (replication) | Custodian (Ops TI) | RM-8, RM-15 | UC1 |
| RM-24 | Manajemen isu data & metrik tata kelola | Integrasi dan Pertukaran Terstandar / Y2Q1–Y2Q2 | TK-9 = 2; TK-10 = 1 (s17); KD-5 = 2 (s15) | Isu data punya pemilik dan SLA | % isu selesai dalam SLA; laporan metrik tata kelola ke KPTI | Kategori & prioritas isu; SLA; ambang eskalasi; daftar metrik | ITSM eksisting [perlu konfirmasi], Dataedo | Hub/UMD + steward | RM-1, RM-14 | UC3 |
| RM-25 | Literasi data & kualitas di sumber | Integrasi dan Pertukaran Terstandar / Y2Q3–Y2Q4 | KD-9 = 1 (s15); TK-14 = 1 (s17) | Kualitas dibangun di titik input | Program pelatihan berjalan; tren isu dari cabang menurun | Kelompok sasaran; kurikulum; indikator unit | Dashboard Kualitas Data (Tableau/QlikSense) | Hub/UMD + Kanwil | RM-24 | UC3 |
| RM-26 | Governed self-service BI & rasionalisasi laporan | Pemanfaatan dan Skala / Y3Q2–Y3Q3 | DB-8 = 1; DB-6 = 2; DB-5 = 2 (s19) | Pengguna membangun di atas data tersertifikasi | % dashboard dari dataset tersertifikasi; laporan duplikat dipensiunkan | Kelompok pengguna; kuota; syarat publikasi; ambang pemakaian minimum | Tableau, QlikSense | Hub/UMD + domain owner | RM-21 | UC3, UC1 |
| RM-27 | Delegasi stewardship ke semua domain | Pemanfaatan dan Skala / Y3Q1–Y3Q2 | TK-2 = 1; TK-3 = 2 (s17) | Federasi bertahap | Semua domain DBB punya owner & steward aktif | Cakupan domain; kriteria delegasi | Dataedo | Hub/UMD + domain owner | RM-24 | UC3 |
| RM-28 | Perluasan MDM & distribusi ke aplikasi | Pemanfaatan dan Skala / Y3Q1–Y3Q2 | s11 Distribusi = 2; DS-2 = 1 (s12) | Golden record dipakai, bukan disimpan | Aplikasi prioritas mengonsumsi golden record via API | Entitas tambahan; kontrak distribusi; SLA sinkronisasi | Mesin MDM, 3scale | Hub/UMD + domain owner | RM-18, RM-15 | UC2, UC1 |
| RM-29 | Policy-as-code & audit akses otomatis | Pemanfaatan dan Skala / Y3Q1–Y3Q2 | KS-8 = 1; KS-5 = 2 (s16) | Kebijakan ditegakkan mesin | Audit akses data Rahasia otomatis; reviu akses berkala terpenuhi | Peristiwa audit wajib; periode reviu akses; aturan alert | Ranger, Atlas | OKI + custodian | RM-11, RM-3 | UC3, UC1 |
| RM-30 | Analitik lanjut di atas gold | Pemanfaatan dan Skala / Y3Q3–Y3Q4 | DB-13 = 2 (s19) | Nilai bisnis dari data terpercaya | Model forecasting & risiko likuiditas di produksi dengan monitoring | Kriteria deployment model; metrik monitoring model | Python/KNIME, Cloudera | Hub/UMD + unit bisnis | RM-20 | UC1 |
| RM-31 | Uji failover rutin & SLA layanan Data Hub | Pemanfaatan dan Skala / Y3Q3–Y3Q4 | DS-7 = 1 (s12); PS-5 = 1 (s18) | Komitmen layanan terukur | Uji failover berkala lulus; laporan SLA ke mitra | Frekuensi uji; target ketersediaan; isi laporan SLA | Cloudera, 3scale | Custodian | RM-23 | UC1, UC2 |
| RM-32 | Evaluasi kematangan & revisi Perdir | Pemanfaatan dan Skala / Y3Q3–Y3Q4 | AD-11 = 2 (s13); TK-4 = 2 (s17) | Kebijakan mengikuti praktik | Asesmen ulang lintas sesi; usulan revisi D1 (peran, retensi, minimisasi) | Metode asesmen; periode reviu kebijakan | — | Hub/UMD + KPTI | RM-27 | UC3 |

Catatan: "s11" sampai "s19" merujuk berkas sesi 11–19. Berkas 11 tidak memakai ID aspek, sehingga aspeknya dirujuk dengan nama.

## 4. Milestone per kuartal per inisiatif

Setiap milestone menjadi bukti kemajuan yang dapat dilaporkan pada laporan triwulanan.

- **RM-1 Juknis peran & forum tata kelola data** (Y1Q1–Y1Q2)
  - Y1Q1: Draf juknis & RACI dibahas forum/KPTI
  - Y1Q2: Juknis terbit; forum TKD bersidang dengan risalah; status pejabat PDP terkonfirmasi
- **RM-2 Penunjukan owner & steward domain pilot** (Y1Q2–Y1Q2)
  - Y1Q2: Owner & steward 2–3 domain pilot ditetapkan dan tercatat di katalog
- **RM-3 Klasifikasi & label PDP di katalog dan tag Ranger** (Y1Q3–Y1Q3)
  - Y1Q3: Kelas & label PDP ditetapkan untuk dataset domain pilot; kebijakan Ranger berbasis tag aktif untuk domain pilot
- **RM-4 Kebijakan retensi & minimisasi (versi awal)** (Y1Q1–Y1Q2)
  - Y1Q1: Jadwal retensi awal kelas prioritas siap sebelum 16 Jan 2027; owner sementara Hub/UMD
  - Y1Q2: Kebijakan retensi & minimisasi disahkan; atribut retensi tercatat di katalog
- **RM-5 Remediasi integrasi Kemnaker (interim)** (Y1Q1–Y1Q2)
  - Y1Q1: Kredensial teks biasa dirotasi dan keluar dari dokumen; akses DB link dibatasi ke elemen PKS
  - Y1Q2: Adendum PKS memuat klausul retensi & audit
- **RM-6 Register PKS, templat dokumen teknis & siklus PKS** (Y1Q2–Y1Q3)
  - Y1Q2: Atribut register & templat dokumen teknis disahkan
  - Y1Q3: % PKS aktif masuk register; status adendum Ps. 78 terlacak
- **RM-7 Inventaris antarmuka & data contract awal** (Y1Q1–Y1Q2)
  - Y1Q1: Inventaris antarmuka upstream/downstream (pemilik, pola, frekuensi) lengkap
  - Y1Q2: Data contract untuk antarmuka prioritas
- **RM-8 Ketahanan platform tahap 1 & keputusan DRC** (Y1Q1–Y1Q3)
  - Y1Q1: Keputusan tertulis status DRC; uji restore sampel gold & metadata dengan berita acara
  - Y1Q2: RPO/RTO per kelas & zona disahkan; backup metadata berjalan
  - Y1Q3: Uji restore berkala kedua lulus
- **RM-9 Rencana kapasitas & monitoring platform** (Y1Q4–Y1Q4)
  - Y1Q4: Dokumen kapasitas per zona; peringatan ambang aktif
- **RM-10 Zona medallion & isolasi lingkungan** (Y1Q4–Y1Q4)
  - Y1Q4: Zona staging/bronze/silver/gold dengan aturan promosi; data non-prod dimasking
- **RM-11 Katalog aktif & lineage** (Y1Q3–Y1Q4)
  - Y1Q3: Atribut metadata wajib terisi untuk domain pilot
  - Y1Q4: Lineage end-to-end untuk alur prioritas; sinkron Atlas–Dataedo berjalan
- **RM-12 Katalog KPI awal & inventaris laporan** (Y1Q1–Y1Q2)
  - Y1Q1: KPI prioritas direksi terdefinisi (definisi, rumus, grain, owner) di Dataedo
  - Y1Q2: Inventaris laporan Tableau/QlikSense lengkap
- **RM-13 Dimensi Program & Kanal + reference data** (Y1Q4–Y2Q1)
  - Y1Q4: Grain & SCD dimensi Program/Kanal disahkan; pemilik kode referensi ditetapkan
  - Y2Q1: Dimensi Program & Kanal di gold dipakai mart prioritas
- **RM-14 CDE & quality gate per lapis** (Y1Q4–Y1Q4)
  - Y1Q4: Daftar CDE & ambang domain pilot; quality gate bronze→silver aktif
- **RM-15 Data Hub (DMZ) & 3scale per PKS** (Y2Q1–Y2Q2)
  - Y2Q1: Desain & application plan per PKS; produk data pilot lulus gerbang klasifikasi
  - Y2Q2: Data Hub go-live; log per transaksi aktif
- **RM-16 Migrasi mitra ke Data Hub & penghentian DB link** (Y2Q3–Y2Q4)
  - Y2Q3: Rencana & urutan migrasi mitra disepakati; mitra gelombang pertama pindah
  - Y2Q4: Akses DB langsung mitra = nol
- **RM-17 Pertukaran file terstandar (SFTP/MFT) via Data Hub** (Y2Q1–Y2Q2)
  - Y2Q1: Aturan validasi & masa simpan file disahkan
  - Y2Q2: Pertukaran file prioritas lewat jalur baku dengan checksum
- **RM-18 Golden record MDM produksi (Peserta & Pemberi Kerja)** (Y2Q1–Y2Q2)
  - Y2Q1: Aturan matching/survivorship & SoR per atribut disahkan (baseline hasil POC)
  - Y2Q2: Golden record Peserta & Pemberi Kerja produksi; antrean steward ber-SLA
- **RM-19 Standar ingest, data contract & SLA freshness** (Y2Q1–Y2Q2)
  - Y2Q1: Target freshness per produk ditetapkan untuk sumber prioritas
  - Y2Q2: Freshness & rekonsiliasi terpantau di dashboard
- **RM-20 Silver Data Vault & gold tersertifikasi** (Y2Q3–Y2Q4)
  - Y2Q3: Domain inti dimuat ke silver Data Vault; kriteria sertifikasi disahkan
  - Y2Q4: Mart prioritas tersertifikasi
- **RM-21 Lapisan metrik bersama & akses BI per klasifikasi** (Y2Q4–Y3Q1)
  - Y2Q4: Definisi metrik bersama untuk KPI prioritas; pemetaan klasifikasi → peran BI
  - Y3Q1: Tableau, QlikSense, dan Data Hub memakai definisi metrik bersama
- **RM-22 Tiering & retensi otomatis di platform** (Y2Q3–Y2Q4)
  - Y2Q3: Batas tier & umur staging disahkan; tiering untuk kelas pertama
  - Y2Q4: Pemusnahan otomatis dengan bukti tercatat
- **RM-23 Klaster DRC warm-standby (gold & serving)** (Y2Q3–Y2Q4)
  - Y2Q3: Kapasitas DRC tersedia; replikasi gold/serving dimulai
  - Y2Q4: Uji failover pertama lulus
- **RM-24 Manajemen isu data & metrik tata kelola** (Y2Q1–Y2Q2)
  - Y2Q1: Kategori, prioritas & SLA isu berlaku
  - Y2Q2: Laporan metrik tata kelola pertama ke KPTI
- **RM-25 Literasi data & kualitas di sumber** (Y2Q3–Y2Q4)
  - Y2Q3: Kurikulum & kelompok sasaran disahkan; angkatan pertama dilatih
  - Y2Q4: Tren isu dari cabang terpantau
- **RM-26 Governed self-service BI & rasionalisasi laporan** (Y3Q2–Y3Q3)
  - Y3Q2: Syarat publikasi & kelompok pengguna berlaku; dataset tersertifikasi dibuka
  - Y3Q3: % dashboard dari dataset tersertifikasi terukur; laporan duplikat dipensiunkan
- **RM-27 Delegasi stewardship ke semua domain** (Y3Q1–Y3Q2)
  - Y3Q1: Kriteria delegasi disahkan; gelombang domain pertama
  - Y3Q2: Semua domain DBB memiliki owner & steward aktif
- **RM-28 Perluasan MDM & distribusi ke aplikasi** (Y3Q1–Y3Q2)
  - Y3Q1: Entitas tambahan & kontrak distribusi disahkan
  - Y3Q2: Aplikasi prioritas mengonsumsi golden record via API
- **RM-29 Policy-as-code & audit akses otomatis** (Y3Q1–Y3Q2)
  - Y3Q1: Peristiwa audit wajib aktif untuk data Rahasia
  - Y3Q2: Reviu akses berkala pertama terpenuhi
- **RM-30 Analitik lanjut di atas gold** (Y3Q3–Y3Q4)
  - Y3Q3: Kriteria deployment & metrik monitoring model disahkan
  - Y3Q4: Model forecasting & risiko likuiditas produksi
- **RM-31 Uji failover rutin & SLA layanan Data Hub** (Y3Q3–Y3Q4)
  - Y3Q3: Uji failover berkala terjadwal
  - Y3Q4: Laporan SLA pertama ke mitra
- **RM-32 Evaluasi kematangan & revisi Perdir** (Y3Q3–Y3Q4)
  - Y3Q3: Asesmen ulang lintas aspek
  - Y3Q4: Usulan revisi D1 (peran, retensi, minimisasi) ke KPTI

## 5. Ringkasan milestone utama per kuartal

| Kuartal | Periode (asumsi) | Milestone utama |
|---|---|---|
| Y1Q1 | Jan–Mar 2027 | RM-1: Draf juknis & RACI dibahas forum/KPTI; RM-4: Jadwal retensi awal kelas prioritas siap sebelum 16 Jan 2027; owner sementara Hub/UMD; RM-5: Kredensial teks biasa dirotasi dan keluar dari dokumen; akses DB link dibatasi ke elemen PKS; RM-7: Inventaris antarmuka upstream/downstream (pemilik, pola, frekuensi) lengkap; RM-8: Keputusan tertulis status DRC; uji restore sampel gold & metadata dengan berita acara; RM-12: KPI prioritas direksi terdefinisi (definisi, rumus, grain, owner) di Dataedo |
| Y1Q2 | Apr–Jun 2027 | RM-1: Juknis terbit; forum TKD bersidang dengan risalah; status pejabat PDP terkonfirmasi; RM-2: Owner & steward 2–3 domain pilot ditetapkan dan tercatat di katalog; RM-4: Kebijakan retensi & minimisasi disahkan; atribut retensi tercatat di katalog; RM-5: Adendum PKS memuat klausul retensi & audit; RM-6: Atribut register & templat dokumen teknis disahkan; RM-7: Data contract untuk antarmuka prioritas; RM-8: RPO/RTO per kelas & zona disahkan; backup metadata berjalan; RM-12: Inventaris laporan Tableau/QlikSense lengkap |
| Y1Q3 | Jul–Sep 2027 | RM-3: Kelas & label PDP ditetapkan untuk dataset domain pilot; kebijakan Ranger berbasis tag aktif untuk domain pilot; RM-6: % PKS aktif masuk register; status adendum Ps. 78 terlacak; RM-8: Uji restore berkala kedua lulus; RM-11: Atribut metadata wajib terisi untuk domain pilot |
| Y1Q4 | Okt–Des 2027 | RM-9: Dokumen kapasitas per zona; peringatan ambang aktif; RM-10: Zona staging/bronze/silver/gold dengan aturan promosi; data non-prod dimasking; RM-11: Lineage end-to-end untuk alur prioritas; sinkron Atlas–Dataedo berjalan; RM-13: Grain & SCD dimensi Program/Kanal disahkan; pemilik kode referensi ditetapkan; RM-14: Daftar CDE & ambang domain pilot; quality gate bronze→silver aktif |
| Y2Q1 | Jan–Mar 2028 | RM-13: Dimensi Program & Kanal di gold dipakai mart prioritas; RM-15: Desain & application plan per PKS; produk data pilot lulus gerbang klasifikasi; RM-17: Aturan validasi & masa simpan file disahkan; RM-18: Aturan matching/survivorship & SoR per atribut disahkan (baseline hasil POC); RM-19: Target freshness per produk ditetapkan untuk sumber prioritas; RM-24: Kategori, prioritas & SLA isu berlaku |
| Y2Q2 | Apr–Jun 2028 | RM-15: Data Hub go-live; log per transaksi aktif; RM-17: Pertukaran file prioritas lewat jalur baku dengan checksum; RM-18: Golden record Peserta & Pemberi Kerja produksi; antrean steward ber-SLA; RM-19: Freshness & rekonsiliasi terpantau di dashboard; RM-24: Laporan metrik tata kelola pertama ke KPTI |
| Y2Q3 | Jul–Sep 2028 | RM-16: Rencana & urutan migrasi mitra disepakati; mitra gelombang pertama pindah; RM-20: Domain inti dimuat ke silver Data Vault; kriteria sertifikasi disahkan; RM-22: Batas tier & umur staging disahkan; tiering untuk kelas pertama; RM-23: Kapasitas DRC tersedia; replikasi gold/serving dimulai; RM-25: Kurikulum & kelompok sasaran disahkan; angkatan pertama dilatih |
| Y2Q4 | Okt–Des 2028 | RM-16: Akses DB langsung mitra = nol; RM-20: Mart prioritas tersertifikasi; RM-21: Definisi metrik bersama untuk KPI prioritas; pemetaan klasifikasi → peran BI; RM-22: Pemusnahan otomatis dengan bukti tercatat; RM-23: Uji failover pertama lulus; RM-25: Tren isu dari cabang terpantau |
| Y3Q1 | Jan–Mar 2029 | RM-21: Tableau, QlikSense, dan Data Hub memakai definisi metrik bersama; RM-27: Kriteria delegasi disahkan; gelombang domain pertama; RM-28: Entitas tambahan & kontrak distribusi disahkan; RM-29: Peristiwa audit wajib aktif untuk data Rahasia |
| Y3Q2 | Apr–Jun 2029 | RM-26: Syarat publikasi & kelompok pengguna berlaku; dataset tersertifikasi dibuka; RM-27: Semua domain DBB memiliki owner & steward aktif; RM-28: Aplikasi prioritas mengonsumsi golden record via API; RM-29: Reviu akses berkala pertama terpenuhi |
| Y3Q3 | Jul–Sep 2029 | RM-26: % dashboard dari dataset tersertifikasi terukur; laporan duplikat dipensiunkan; RM-30: Kriteria deployment & metrik monitoring model disahkan; RM-31: Uji failover berkala terjadwal; RM-32: Asesmen ulang lintas aspek |
| Y3Q4 | Okt–Des 2029 | RM-30: Model forecasting & risiko likuiditas produksi; RM-31: Laporan SLA pertama ke mitra; RM-32: Usulan revisi D1 (peran, retensi, minimisasi) ke KPTI |

## 6. Daftar dependensi

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
| RM-21 Lapisan metrik bersama & akses BI per klasifikasi | RM-12 Katalog KPI awal & inventaris laporan, RM-20 Silver Data Vault & gold tersertifikasi, RM-3 Klasifikasi & label PDP di katalog dan tag Ranger |
| RM-22 Tiering & retensi otomatis di platform | RM-4 Kebijakan retensi & minimisasi (versi awal), RM-11 Katalog aktif & lineage |
| RM-23 Klaster DRC warm-standby (gold & serving) | RM-8 Ketahanan platform tahap 1 & keputusan DRC, RM-15 Data Hub (DMZ) & 3scale per PKS |
| RM-24 Manajemen isu data & metrik tata kelola | RM-1 Juknis peran & forum tata kelola data, RM-14 CDE & quality gate per lapis |
| RM-25 Literasi data & kualitas di sumber | RM-24 Manajemen isu data & metrik tata kelola |
| RM-26 Governed self-service BI & rasionalisasi laporan | RM-21 Lapisan metrik bersama & akses BI per klasifikasi |
| RM-27 Delegasi stewardship ke semua domain | RM-24 Manajemen isu data & metrik tata kelola |
| RM-28 Perluasan MDM & distribusi ke aplikasi | RM-18 Golden record MDM produksi (Peserta & Pemberi Kerja), RM-15 Data Hub (DMZ) & 3scale per PKS |
| RM-29 Policy-as-code & audit akses otomatis | RM-11 Katalog aktif & lineage, RM-3 Klasifikasi & label PDP di katalog dan tag Ranger |
| RM-30 Analitik lanjut di atas gold | RM-20 Silver Data Vault & gold tersertifikasi |
| RM-31 Uji failover rutin & SLA layanan Data Hub | RM-23 Klaster DRC warm-standby (gold & serving) |
| RM-32 Evaluasi kematangan & revisi Perdir | RM-27 Delegasi stewardship ke semua domain |

**Jalur kritis (rantai ketergantungan terpanjang, 8 langkah, Y1Q1–Y3Q3):** RM-1 (Y1Q1–Y1Q2) → RM-2 (Y1Q2) → RM-11 (Y1Q3–Y1Q4) → RM-14 (Y1Q4) → RM-18 (Y2Q1–Y2Q2) → RM-20 (Y2Q3–Y2Q4) → RM-21 (Y2Q4–Y3Q1) → RM-26 (Y3Q2–Y3Q3). Jalur ini memuat tiga overlap disengaja (RM-1→RM-2, RM-11→RM-14, RM-20→RM-21) sehingga tidak ada cadangan waktu: keterlambatan satu kuartal pada juknis peran (RM-1) menggeser golden record, gold tersertifikasi, lapisan metrik, dan self-service dengan besaran yang sama. Cabang RM-20 → RM-30 (Y3Q3–Y3Q4) berakhir paling akhir di Y3Q4.

**Rantai kedua (7 langkah, Y1Q1–Y3Q4):** RM-1 (Y1Q1–Y1Q2) → RM-2 (Y1Q2) → RM-3 (Y1Q3) → RM-10 (Y1Q4) → RM-15 (Y2Q1–Y2Q2) → RM-23 (Y2Q3–Y2Q4) → RM-31 (Y3Q3–Y3Q4). Rantai ini memiliki cadangan dua kuartal antara RM-23 dan RM-31; cadangan tersebut dapat menyerap keterlambatan keputusan DRC.

**Overlap disengaja** (inisiatif mulai sebelum prasyarat selesai):

| Prasyarat → inisiatif | Alasan |
|---|---|
| RM-1 → RM-4 | PP 33/2026 berlaku Y1Q1; retensi disusun paralel dengan juknis, owner sementara Hub/UMD |
| RM-1 → RM-2 | Owner pilot ditunjuk setelah draf juknis dibahas forum (Y1Q1), tanpa menunggu juknis terbit |
| RM-1 → RM-6 | Register PKS mulai setelah draf RACI tersedia (Y1Q1) |
| RM-11 → RM-14 | CDE domain pilot didefinisikan saat katalog aktif; lineage dirampungkan pada kuartal yang sama |
| RM-20 → RM-21 | Lapisan metrik dibangun di atas mart pertama yang tersertifikasi (Y2Q3), sebelum seluruh gold selesai |

## 7. Tonggak eksternal

| Tonggak | Waktu / kuartal | Inisiatif terdampak | Catatan konflik |
|---|---|---|---|
| PP 33/2026 berlaku (retensi Ps. 75, minimisasi Ps. 59) | 16 Jan 2027 → Y1Q1 [perlu konfirmasi] | RM-4, RM-22, RM-5, RM-17 | **Kunci waktu, konflik.** Bila Tahun 1 dimulai Jan 2027 [perlu konfirmasi], PP 33/2026 berlaku pada pekan ketiga Y1Q1. Karena itu RM-4 dimulai Y1Q1 sebagai langkah pertama, dan penyusunan jadwal retensi untuk kelas prioritas perlu dimulai sebelum tanggal berlaku. Owner retensi (TK-12 = 1) belum ada, sehingga kepemilikan sementara dipegang Hub/UMD sampai RM-2 menetapkan owner domain. Otomasi retensi (RM-22) baru berjalan di Tahun 2. |
| Target POC MDM | Okt 2026 (sebelum Y1Q1) | RM-18, RM-28 | **Konflik.** Target sudah lewat dan statusnya belum terkonfirmasi (11-Klarifikasi). Bila manajemen mendorong produksi segera, aturan "owner domain sebelum golden record" dilanggar. Usulan: hasil POC dibekukan sebagai baseline, dan produksi dijadwalkan Y2Q1–Y2Q2 (RM-18). |
| Data Hub (bangun baru, DMZ) | Rencana Y2Q2; jadwal BPJS [perlu konfirmasi] | RM-15, RM-16, RM-28 | **Konflik potensial.** Bila pembangunan berjalan sebelum klasifikasi (RM-3) dan register PKS (RM-6), kontrol akses berbasis kelas tidak dapat ditegakkan. Usulan: klasifikasi minimum untuk produk data pilot sebagai gerbang go-live. |
| Status DRC | Belum terselesaikan; keputusan Y1Q1 | RM-8, RM-23, RM-31, RM-15 | **Konflik.** D3-01, R1, dan D2 hlm. 50 bertentangan. Data Hub (go-live Y2Q2) akan berjalan tanpa DR platform sampai RM-23 selesai (Y2Q4). Keputusan DRC dikunci di Y1Q1 (RM-8). |
| Penutupan DB link Kemnaker | Y1Q1 (interim) vs Y2Q4 (final) | RM-5, RM-16 | Penutupan penuh menunggu Data Hub. Selama masa itu berlaku kontrol interim RM-5 (rotasi kredensial, pembatasan akses). |
| Integrasi Taspen/Asabri | 2029 (s11, tidak dinilai) | RM-28 | Di luar horizon; dipantau sebagai masukan perluasan MDM. |

## 8. Matriks cakupan use case TOR

| Use case | Tahun 1 | Tahun 2 | Tahun 3 | Jumlah |
|---|---|---|---|---|
| UC1 Kesiapan infrastruktur ESDP | RM-3, RM-8, RM-9, RM-10, RM-11, RM-13 | RM-15, RM-18, RM-19, RM-20, RM-21, RM-22, RM-23 | RM-26, RM-28, RM-29, RM-30, RM-31 | 18 |
| UC2 API & pertukaran data | RM-5, RM-6, RM-7 | RM-15, RM-16, RM-17, RM-19 | RM-28, RM-31 | 9 |
| UC3 Tata kelola data | RM-1, RM-2, RM-3, RM-4, RM-5, RM-6, RM-11, RM-12, RM-13, RM-14 | RM-18, RM-21, RM-22, RM-24, RM-25 | RM-26, RM-27, RM-29, RM-32 | 19 |

Celah cakupan:
- UC2 tipis di Tahun 1. Isinya hanya persiapan (RM-5, RM-6, RM-7), karena Data Hub sengaja ditunda sampai klasifikasi siap.
- ODS/operational serving (AD-6 = 1) belum memiliki inisiatif. Kebutuhannya bergantung pada konfirmasi pola konsumsi operasional.
- Kapasitas jangka panjang terkait volume peserta hanya dicakup RM-9 di tingkat rencana.

## 9. Pertanyaan terbuka yang menghambat pengurutan

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
