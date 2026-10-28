---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Keamanan Data
sesi: 6
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.3, 5.4, 6]
status: draft-diskusi
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, 00-Analisis-Dokumen-BPJS.md, bpjstk_tender_inventory.csv, 12-Integrasi.md, 13-Arsitektur-Data.md, 14-Pemodelan-Data.md, 15-Metadata-Kualitas.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 94 = cetak '- 93 -')."
---

# Sesi 6 — Keamanan Data

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI (kebijakan, kondisi yang seharusnya). **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (paparan kondisi saat ini/as-is). "hlm." merujuk pada nomor halaman PDF; "Ps. N(M)" = Pasal N ayat (M).

> **Kerangka sesi.** Konsisten dengan sesi 1–5: ESDP adalah platform poros, dibangun dengan Opsi B di atas Cloudera CDP on-prem eksisting (zona staging | lake | DWH | serving, dipetakan ke landing/bronze/silver/gold). Integrasi adalah komponen ESDP: **upstream** = sumber → platform; **downstream** = platform → pengguna/mitra. Pertukaran eksternal melalui web service dan SFTP (Ps. 66(3)); akses langsung ke database dihentikan bertahap ([[12-Integrasi]]). Metadata memakai katalog federasi Atlas + Dataedo ([[15-Metadata-Kualitas]]). Blueprint menetapkan **kontrol dan parameter wajib**, bukan nilainya.

## 1. Ringkasan
- **Kebijakan keamanan sangat lengkap; bukti praktiknya di level data hampir tidak ada.** D1 Bab V mengatur SMKI dengan 16 area kontrol (Ps. 49, hlm. 39–40), klasifikasi data tiga tingkat Biasa/Terbatas/Rahasia beserta kontrol minimumnya (Lampiran III A.2, hlm. 93–94), matriks kontrol akses data CRUD per elemen (Lampiran III B.4–5, hlm. 101), kriptografi AES ≥256 bit dengan key management terpusat (Ps. 57(3)–(4), hlm. 45), dan isi log minimum (Lampiran III D, hlm. 107).
- **D2 hanya menyebut "Data Security" sebagai lapisan horizontal pada diagram** (D2 hlm. 20), tanpa uraian kondisi. Yang dijelaskan justru risikonya: kebocoran pada transfer eksternal (D2 hlm. 21–22), serta data lake dan DWH dalam satu environment sehingga dampak serangan lebih besar (D2 hlm. 21).
- **Pertukaran eksternal paling matang.** Ps. 66 mengatur persetujuan OKI, PKS/NDA, whitelist IP, dan teknologi web service/SFTP host-to-host (D1 hlm. 50–51). Praktiknya sejalan: API untuk pertukaran eksternal dan SFTP untuk bulk dengan K/L (D2 hlm. 21), ditambah lapisan WAF/API Security (D2 hlm. 37) dan tender API security 2024 (publik).
- **Celah utama untuk ESDP:** klasifikasi belum dipetakan ke kategori UU 27/2022 (data pribadi umum vs spesifik); belum ada bukti klasifikasi tercatat sebagai metadata (tag); kebijakan akses terpusat di Cloudera (Ranger) dan masking/tokenisasi tidak disebut; audit akses data (siapa membaca apa) belum terlihat; Ps. 3(4) menyerahkan PDP ke peraturan direksi tersendiri yang belum kami terima [perlu konfirmasi].
- **Skor usulan: Keamanan Data ±1,7** (rentang 1–3).
- **Rekomendasi (usulan, berbasis perbandingan):** **Opsi C — kontrol berbasis klasifikasi (tag-based)**. Klasifikasi dan label PDP ditetapkan di katalog (Dataedo/Atlas), di-propagasi sebagai tag Atlas, lalu ditegakkan oleh Ranger dalam bentuk kombinasi RBAC (peran dari matriks akses D1) dan ABAC (atribut klasifikasi, unit, tujuan). Masking dinamis di serving, tokenisasi NIK/no. KPJ di silver, enkripsi at-rest dan in-transit di semua zona, dan audit terpusat.

---

## 2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Lapisan keamanan data | Digambar sebagai lapisan horizontal "Data Security" | D2 hlm. 20 | Tidak ada uraian kapabilitas di D2 hlm. 21–22 |
| Segregasi environment platform | Data lake dan DWH di Cloudera, satu environment | D2 hlm. 21 | D2 sendiri mencatat risiko dampak serangan yang lebih besar |
| Pertukaran eksternal | API; bulk via SFTP untuk PKS dengan K/L | D2 hlm. 21 | Sejalan dengan Ps. 66(3) |
| Risiko transfer eksternal | Kebocoran, keterlambatan, kegagalan transfer akibat kontrol keamanan yang kurang baik | D2 hlm. 21–22 | Diakui sebagai risiko oleh BPJS sendiri |
| Data collaboration | Platform eksternal beragam, sesuai mitra | D2 hlm. 22 | Kontrol tidak seragam antarmitra |
| Perimeter aplikasi | Load balancer → firewall → WAF / API Security → API Management | D2 hlm. 37 (via 00-Analisis) | Tender API security 2024 (publik) |
| Organisasi keamanan | CISO, ISM; Asdep Keamanan TI; Asdep Platform Teknologi & Keamanan | D1 Ps. 50 (hlm. 40); D2 hlm. 51 | Belum ada peran keamanan data di Unit Manajemen Data [perlu konfirmasi] |
| Klasifikasi data tercatat | Tidak ada bukti inventaris klasifikasi per elemen | – | [perlu konfirmasi] |
| Kontrol akses di Cloudera | Tidak ada bukti (Ranger/Sentry, Kerberos, LDAP) | – | Lazimnya bawaan CDP [perlu konfirmasi] |
| Masking/tokenisasi | Tidak disebut | – | Juga dicatat di 00-Analisis |
| Data uji (non-prod) | Kebijakan otorisasi data operasional di lingkungan uji ada | D1 Ps. 61 (hlm. 47) | Praktik masking data uji tidak diketahui |
| Audit/log akses data | Kebijakan log ada; bukti audit akses data platform tidak ada | D1 Lampiran III D (hlm. 106–107) | [perlu konfirmasi] |

## 3. Kondisi yang seharusnya (D1)
- **Ps. 2(2)c, Ps. 3(1)d–e:** keamanan informasi dan PDP termasuk ruang lingkup; **Ps. 3(4):** PDP diatur dalam peraturan direksi tersendiri (D1 hlm. 5–6).
- **Ps. 31(2)c:** pengelolaan Katalog Data wajib memastikan keamanan berdasarkan jenis klasifikasi data (D1 hlm. 29).
- **Ps. 49:** SMKI berbasis kerahasiaan, integritas, dan ketersediaan; mencakup pengendalian akses, kriptografi, pengamanan operasional, keamanan interoperabilitas, dan kepatuhan (D1 hlm. 39–40).
- **Ps. 56:** OKI mengendalikan hak akses insan Badan dan pihak ketiga; tata cara di Lampiran III B (D1 hlm. 44).
- **Ps. 57(3)–(4):** enkripsi AES/SHA dengan key ≥256 bit; HTTPS untuk aplikasi publik; key disimpan di key management terpusat dengan authentication, authorization, dan reviu (D1 hlm. 45).
- **Ps. 61:** persyaratan keamanan di setiap pengembangan; otorisasi pemakaian data operasional di lingkungan uji; perlindungan data saat pengembangan dan pengujian (D1 hlm. 46–47).
- **Ps. 63, 64:** persyaratan keamanan dalam kontrak pihak ketiga; manajemen insiden keamanan informasi (D1 hlm. 48–49).
- **Ps. 66(1)–(5):** keamanan interoperabilitas: persetujuan OKI untuk akses eksternal (prod/dev/test); lampiran PKS, IP address, kebutuhan, NDA; host-to-host via web service dan SFTP; teknologi lain wajib dikaji; web service wajib otentikasi, otorisasi, kontrol akses, perimeter, dan whitelist IP (D1 hlm. 50–51).
- **Ps. 67(1)c, g:** pengendalian kerahasiaan data pribadi; audit sistem informasi (D1 hlm. 51–52).
- **Ps. 68(3), 71(1)d:** kolaborasi data memperhatikan PDP dan prinsip kerahasiaan; PKS wajib memuat keamanan dan kerahasiaan data (D1 hlm. 54, 56).
- **Lampiran II (DWBI):** arsitektur DWBI wajib mempertimbangkan keamanan akses, keamanan data, dan privasi (D1 hlm. 78); prinsip transparansi dengan memperhatikan keamanan data (D1 hlm. 76).
- **Lampiran III A.2:** klasifikasi diusulkan Unit Manajemen Data bersama UPTI dan Unit Bisnis; Biasa (backup), Terbatas (backup, kontrol akses DB/storage, medium pertukaran terlindung), Rahasia (ditambah enkripsi atau kontrol setara, serta enkripsi data yang dipertukarkan) (D1 hlm. 93–94).
- **Lampiran III B:** user ID unik, reviu hak akses minimal setahun sekali, PAM, pembatasan via role, kontrol read/write/delete/execute (D1 hlm. 97–99); **matriks kontrol akses data** CRUD per elemen per peran disusun Unit Manajemen Data dan dipakai Unit Pengembangan dan Operasional TI (D1 hlm. 100–101).
- **Lampiran III D:** pemisahan environment dev/test/staging/prod, termasuk DWBI (D1 hlm. 105–106); log minimal user ID, IP, aktivitas, waktu; administrator tidak boleh mengubah atau menghapus log; VA bulanan, pentest tahunan (D1 hlm. 107).

**Catatan regulasi eksternal.** UU 27/2022 (dirujuk di konsiderans D1 hlm. 2) membedakan data pribadi umum dan spesifik (antara lain data kesehatan dan data keuangan pribadi), mewajibkan dasar pemrosesan, pembatasan tujuan, keamanan, notifikasi kegagalan pelindungan, dan Pejabat Pelindungan Data Pribadi. Pemetaan klasifikasi D1 ke kategori ini belum terlihat [perlu konfirmasi isi Perdir PDP].

## 4. Tabel penilaian — Keamanan Data

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal.

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| KS-1. Tata kelola & organisasi keamanan data | Pemilik kebijakan, peran keamanan data di UMD, tautan ke data owner | Ps. 49–50 (hlm. 39–42) | CISO/ISM dan Asdep Keamanan TI ada (D2 hlm. 51); peran keamanan data di UMD tidak terlihat | Jembatan OKI–UMD–data owner | 2 |
| KS-2. Klasifikasi data | Klasifikasi per elemen, tercatat di katalog sebagai tag, ditinjau berkala | Lampiran III A.2 (hlm. 93–94); Ps. 31(2)c (hlm. 29) | Tidak ada bukti inventaris klasifikasi | Implementasi & tagging | 2 (batas kebijakan) [perlu konfirmasi] |
| KS-3. Pemetaan PDP (UU 27/2022) | Label data pribadi umum/spesifik, dasar pemrosesan, tujuan | Ps. 3(4) dirujuk ke Perdir tersendiri (hlm. 6); Ps. 67(1)c (hlm. 52) | Tidak ada bukti | Pemetaan klasifikasi ↔ PDP | 1 [perlu konfirmasi] |
| KS-4. Kontrol akses (RBAC/ABAC) | Matriks CRUD per elemen, ditegakkan terpusat di platform; least privilege | Ps. 56; Lampiran III B (hlm. 97–101) | Tidak ada bukti matriks maupun Ranger | Matriks & penegakan di ESDP | 2 (batas kebijakan) |
| KS-5. Akses istimewa & reviu akses | PAM, reviu periodik, pencabutan saat mutasi | Lampiran III B.1c (hlm. 98–99) | Tidak ada bukti untuk platform data | Bukti pelaksanaan | 2 (batas kebijakan) |
| KS-6. Masking & tokenisasi | Masking dinamis per peran; tokenisasi identitas; data uji ter-masking | Ps. 61 huruf c, f (hlm. 47); "kontrol lain sesuai risiko" (hlm. 94) | Tidak disebut | Belum ada | 1 |
| KS-7. Enkripsi & key management | At-rest dan in-transit; KMS terpusat | Ps. 57(3)–(4) (hlm. 45) | HTTPS/WAF di aplikasi (D2 hlm. 37); enkripsi di Cloudera tidak terlihat | Bukti at-rest di HDFS/Ozone & KMS | 2 [perlu konfirmasi] |
| KS-8. Audit & monitoring akses data | Log akses data terpusat, tidak bisa diubah admin, dianalisis | Lampiran III D.1g (hlm. 106–107) | Tidak ada bukti audit akses data | Audit tingkat data | 1 [perlu konfirmasi] |
| KS-9. Keamanan pertukaran eksternal | Persetujuan OKI, PKS/NDA, web service/SFTP, whitelist IP, enkripsi | Ps. 66(1)–(5) (hlm. 50–51); Ps. 71(1)d (hlm. 56) | API & SFTP berjalan (D2 hlm. 21); WAF/API Security (D2 hlm. 37); risiko kebocoran diakui (D2 hlm. 21–22) | Konsistensi antarmitra, akses DB langsung | 3 |
| KS-10. Berbagi aman ke downstream internal | Akses ke data mart/BI sesuai klasifikasi, row/column-level security | Lampiran II DWBI (hlm. 76, 78) | Tableau/QlikSense, 17 dashboard (D2 hlm. 22); kontrol tidak diuraikan | Kontrol di serving & BI | 2 [perlu konfirmasi] |
| KS-11. Segregasi environment & data non-prod | Lake/DWH/serving terpisah secara logis; data prod tidak mentah di non-prod | Lampiran III D.1c (hlm. 105–106); Ps. 61 (hlm. 47) | Lake dan DWH satu environment (D2 hlm. 21) | Segmentasi zona | 1 |

**Rata-rata Keamanan Data: ±1,7.**

## 5. Opsi pendekatan keamanan data di ESDP

| Opsi | Deskripsi |
|---|---|
| A. Perimeter + kontrol per sistem | Pola saat ini: firewall/WAF, akses diatur per aplikasi, database, dan tool BI masing-masing |
| B. RBAC terpusat berbasis objek | Ranger di CDP; kebijakan per database/tabel/kolom per peran; masking manual per tabel |
| C. Kontrol berbasis klasifikasi (tag-based, RBAC + ABAC) | Klasifikasi & label PDP ditetapkan di katalog, dipropagasi sebagai tag Atlas, ditegakkan Ranger (tag-based policy, row filter, masking dinamis); tokenisasi identitas di silver; enkripsi & audit terpusat |
| D. Platform privasi/keamanan data pihak ketiga | Produk khusus (discovery, tokenisasi, policy engine) di atas seluruh platform |

**Perbandingan (++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria | A | B | C | D |
|---|---|---|---|---|
| Memenuhi Ps. 31(2)c (keamanan berdasarkan klasifikasi) | −− | 0 | ++ | ++ |
| Menegakkan matriks CRUD per elemen (Lampiran III B.5) | − | + | ++ | + |
| Kesiapan PDP (label, minimisasi, masking) | −− | 0 | + | ++ |
| Skalabilitas saat dataset bertambah | −− | − (kebijakan per objek meledak) | ++ | + |
| Reuse investasi (CDP, Atlas, Dataedo) | + | ++ | ++ | − |
| Audit terpusat | − | + | ++ | + |
| Biaya & risiko pengadaan | ++ | + | + | −− |
| Kompleksitas implementasi | ++ | + | 0 (butuh disiplin klasifikasi) | − |

**Rekomendasi (usulan): Opsi C.** Opsi C satu-satunya yang langsung memenuhi Ps. 31(2)c dan Lampiran III A.2 tanpa pengadaan baru, karena Ranger dan Atlas lazimnya sudah termasuk di CDP [perlu konfirmasi lisensi dan versi]. Opsi C juga menyambung ke katalog federasi sesi 5: tag klasifikasi yang ditetapkan sekali di katalog menjadi dasar kebijakan akses. Opsi B dipakai sebagai langkah antara di Tahun 1 untuk domain yang belum terklasifikasi. Opsi D baru dipertimbangkan jika Perdir PDP menuntut kapabilitas yang tidak dapat dipenuhi CDP (mis. discovery otomatis lintas Oracle/MariaDB atau format-preserving tokenization).

## 6. Kontrol per zona ESDP (usulan Blueprint)

| Zona / lapis | Kontrol utama | Akses | Catatan |
|---|---|---|---|
| Upstream (sumber → staging) | TLS untuk Qlik Replicate/Talend; akun servis tanpa login interaktif; secret di vault | Hanya akun pipeline | Kurangi akses langsung ke DB sumber |
| Staging / landing | Enkripsi at-rest; retensi pendek | Hanya pipeline dan operator | Data mentah, termasuk PII |
| Lake / bronze | Enkripsi at-rest (KMS); tag klasifikasi otomatis saat ingest | Data engineer terbatas, PAM | Raw tidak boleh dikonsumsi pengguna bisnis |
| DWH / silver | Tokenisasi NIK, no. KPJ, rekening; vault token terpisah | Steward & analis per domain (ABAC) | Detokenisasi hanya dengan otorisasi tercatat |
| Serving / gold, data mart | Masking dinamis, row-level filter (mis. per kantor wilayah/cabang) | Pengguna bisnis, BI | Tableau/QlikSense via akun personal atau propagasi identitas [perlu konfirmasi] |
| Downstream eksternal | Web service/SFTP (Ps. 66(3)); minimisasi elemen sesuai PKS; enkripsi file Rahasia | Mitra per PKS, whitelist IP | Persetujuan OKI (Ps. 66(1)b) |
| Non-prod | Data sintetis atau ter-masking | Tim pengembang | Ps. 61 huruf c, f |

## 7. Berbagi aman ke downstream

| Pola | Penerima | Kontrol wajib |
|---|---|---|
| Verifikasi data (Ps. 69(2)) | Dukcapil, mitra perbankan/kanal | Jawaban ya/tidak atau skor; tidak mengembalikan data mentah |
| Pertukaran data (Ps. 69(4)) | DJP, Kemnaker, BPJS Kesehatan, K/L | Elemen sesuai PKS (Ps. 71(1)c); SFTP terenkripsi atau API dengan OAuth/mTLS [perlu konfirmasi standar]; log per transaksi |
| Data sebagai layanan (Ps. 69(3)) | Mitra Non-KL | Agregat/insight; ambang minimum agregasi; tanpa data pribadi |
| Internal (BI, self-service) | Unit bisnis, kanwil, kancab | Masking dan row filter dari tag; akses melalui katalog dengan alur permintaan |

## 8. Parameter wajib (tanpa nilai)
1. Skema klasifikasi: tingkat D1 × label PDP (umum/spesifik) × tingkat kritikalitas; pemilik penetapan; frekuensi reviu.
2. Matriks akses: peran, atribut ABAC (unit, wilayah, tujuan, klasifikasi), hak CRUD per elemen.
3. Aturan masking/tokenisasi per tipe elemen; siapa boleh detokenisasi.
4. Enkripsi: algoritma, panjang key, rotasi key, lokasi KMS.
5. Audit: event yang dicatat, retensi log, frekuensi reviu, ambang alarm.
6. Pertukaran eksternal: protokol, otentikasi, enkripsi payload/file, retensi file SFTP, SLA pencabutan akses.
7. Reviu hak akses (D1 menetapkan minimal setahun sekali), SLA pencabutan saat mutasi.
8. Retensi dan pemusnahan data pribadi; notifikasi kegagalan pelindungan.

## 9. Batas peran

| Peran | Tanggung jawab keamanan data |
|---|---|
| OKI (CISO, ISM) | Kebijakan SMKI, persetujuan akses eksternal, insiden (Ps. 50, 64, 66) |
| Unit Manajemen Data | Usulan klasifikasi, matriks kontrol akses data, tag di katalog (Lampiran III A.2, B.4) |
| Unit Bisnis (data owner) | Menyetujui klasifikasi dan permintaan akses domainnya |
| Unit Pengembangan TI | Menerapkan matriks di aplikasi; data uji aman (Ps. 61) |
| Unit Operasional TI | Operasional Ranger/KMS/log, pemisahan environment, PAM |
| Pejabat PDP | Kepatuhan UU 27/2022 [perlu konfirmasi apakah sudah ditunjuk] |
| Unit Pemrakarsa PKS | Memastikan klausul keamanan dan elemen data (Ps. 71) |

## 10. Keterkaitan dengan Blueprint
- **3.1–3.3:** bagian 2–4. **3.4 / 5.3–5.4:** bagian 6–8.
- **6 (roadmap) — usulan tema:**
  - **Tahun 1 — Fondasi:** klasifikasi dan label PDP untuk 5 shared dimension; Ranger RBAC dasar dan enkripsi at-rest; audit terpusat; pemisahan zona.
  - **Tahun 2 — Layanan:** tag-based policy dan masking dinamis; tokenisasi identitas; standardisasi pertukaran eksternal di web service/SFTP; matriks akses lintas domain.
  - **Tahun 3 — Optimasi:** ABAC penuh; analitik perilaku akses; evaluasi kebutuhan platform privasi khusus.

## 11. Pertanyaan terbuka untuk BPJS
1. Apakah Perdir PDP (Ps. 3(4)) sudah terbit, dan apa isinya terkait klasifikasi dan pemrosesan?
2. Apakah klasifikasi Biasa/Terbatas/Rahasia sudah diterapkan per elemen data, dan di mana dicatat?
3. Apakah matriks kontrol akses data (Lampiran III B.4) sudah disusun?
4. Apakah Ranger, Kerberos, dan enkripsi at-rest aktif di Cloudera?
5. Bagaimana akses Tableau/QlikSense ke data dikendalikan (akun servis atau identitas pengguna)?
6. Apakah ada masking atau tokenisasi, termasuk untuk data uji?
7. Bagaimana log akses data dikumpulkan dan siapa yang meninjaunya?
8. Berapa mitra yang masih memakai akses database langsung, dan apa rencana migrasinya?
9. Apa lingkup tender API security 2024 dan apakah mencakup layanan pertukaran data?

## 12. Poin diskusi (ikpu)
- [ ] Sepakati Opsi C (kontrol berbasis klasifikasi, RBAC + ABAC di Ranger) dengan Opsi B sebagai langkah antara.
- [ ] Sepakati pemetaan klasifikasi D1 ke label PDP sebagai prasyarat; perlu Perdir PDP.
- [ ] Tetapkan pilot: 5 shared dimension, tokenisasi NIK/no. KPJ di silver.
- [ ] Sepakati kontrol per zona (bagian 6) dan parameter wajib (bagian 8) tanpa nilai.
- [ ] Putuskan apakah segregasi lake/DWH cukup logis (Ranger, namespace) atau perlu fisik.
- [ ] Validasi skor bertanda [perlu konfirmasi], terutama KS-2, KS-3, KS-7, KS-8.

## Lampiran — Deskripsi diagram (gaya Excalidraw)
Diagram alur kiri → kanan: **Sumber** (SMILE, Oracle, MariaDB) → panah "TLS / akun servis" → kotak ESDP dengan empat zona berjajar (**Staging | Lake/bronze | DWH/silver | Serving/gold**), tiap zona diberi ikon gembok (enkripsi KMS). Di atas zona: pita **Katalog (Dataedo + Atlas) → tag klasifikasi**, dengan panah turun ke pita **Ranger (RBAC + ABAC, masking, row filter)** yang membentang di bawah semua zona. Label "tokenisasi" di silver dan "masking dinamis" di serving. Di kanan: dua cabang downstream: **Internal** (Tableau/QlikSense, kanwil/kancab) dan **Eksternal** (Dukcapil, DJP, Kemnaker, BPJS Kesehatan) melalui gerbang **Web service / SFTP — persetujuan OKI, whitelist IP**. Di dasar diagram: pita **Audit terpusat (log tak dapat diubah)**. Akses DB langsung digambar putus-putus dengan tanda silang ("dihentikan bertahap").
