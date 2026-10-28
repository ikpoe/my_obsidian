# Ekstraksi Informasi — Rekaman Diskusi BPJS Ketenagakerjaan, 27 Okt 2026

Sumber: `transkrip-pagi.md` (pagi.ogg, durasi 2:11:48) dan `transkrip-siang.md` (siang.ogg, durasi 0:52:22).
Transkripsi otomatis memakai faster-whisper model *small* (CPU, bahasa id). Kualitasnya **rendah–sedang**: istilah teknis sering salah dengar, banyak bagian berulang/halusinasi (mis. "baru, baru…", "Loh ga?", "Kudu" berulang), tidak ada pemisahan pembicara.
Rujukan waktu ditulis [P hh:mm:ss] untuk pagi dan [S hh:mm:ss] untuk siang.
Penafsiran istilah yang salah dengar: talent/talun → **Talend**; click/klik → **Qlik (Replicate)**; kudu → **Apache Kudu**; parkit/party → **Parquet**; Kodera/klaudera → **Cloudera**; data edu / data IDO / datedoh → **Dataedo**; FPI/G2/API G2 → kemungkinan **API Gateway**; Impra → **Infra(struktur)**; kafa/kakak → kemungkinan **Kafka**; DGP → kemungkinan DJP [tidak pasti]; "pak Iqlas/i-class" → ikhlas (SA kita).
Label: **(a)** fakta baru kondisi saat ini · **(b)** konfirmasi/kontradiksi asumsi · **(c)** keputusan/permintaan/ekspektasi stakeholder · **(d)** pertanyaan terbuka/action item.

## Pembicara / peran (perkiraan, tidak ada diarisasi)
- **Tim konsultan (kita)**: memandu sesi, menjelaskan profiling, PDP, medallion, metadata, data hub. Nama "pak Iqlas" disebut sebagai arsitek/"engineer kita" yang sedang ada tugas lain [S 00:50:40–00:50:46], dan sebagai pihak yang diminta membuat usulan desain [P 01:27:01–01:27:09]. Disebut juga bahwa ia pernah membantu di Kemenkes/Dukcapil [P 01:27:39–01:27:47].
- **Tim BPJS TK (data/big data, kepesertaan)**: menjelaskan Cloudera, Qlik, Talend, Kudu, Dataedo, kode TK/kode perusahaan, JKP.
- Nama yang terdengar (ejaan tidak pasti): Pak Musakim/Pak Mus, Pak Betyan, Pak Stoli, Pak Aswak/Pak Astar/Pak Asif, Pak Jil, Mas Aji, Pak David, Pak Widi (disebut pernah memimpin tim data saat implementasi Cloudera) [S 00:50:24–00:50:42; P 00:26:27; P 00:44:02; P 01:40:31; P 02:05:11]. "Pak Makwi/Mok-Pik" disebut sebagai pihak yang bertanya/punya API di infra [S 00:11:47; P 01:09:54] [tidak jelas].
- Ada pihak yang tampaknya pemimpin proyek/PM di sisi kita, yang mendorong timeline dan komitmen [P 02:08:57–02:09:31; S 00:41:12–00:42:08].

---

## 1. Master Data (MDM)
- (a) **Identitas peserta**: peserta mengenal **KPJ**, tetapi secara sistem setiap orang diberi ID internal sendiri, yaitu **kode TK** [P 00:34:11–00:34:28]. Perusahaan diberi **NPP** (secara sistem disebut "kode perusahaan") [P 00:34:45–00:34:48]. Gabungan kode perusahaan + kode TK membentuk **kode kepesertaan** [P 00:34:52–00:34:55]. Sistemnya **dibangun sendiri (custom)**, bukan SAP/Oracle, dan sudah lama dipakai [P 00:35:00–00:35:13].
- (a) Untuk **BPU** ada pengelompokan "**wadah**" yang berperan seperti perusahaan [P 00:36:25–00:36:44].
- (a) Ketika peserta pindah perusahaan atau cabang, **kode kepesertaan berubah**, sedangkan nomor kartu bisa tetap. Peserta tidak melihat perubahan itu, tetapi BPJS tahu dari penanda di kode kepesertaan [P 01:09:56–01:10:44]. Mutasi perorangan terjadi setiap hari [P 01:09:48–01:09:52].
- (a) **Merger perusahaan**: tidak ada mekanisme baku (mis. tidak lewat PHK). Pembina di cabang berkoordinasi ke kantor pusat (bagian kepesertaan/"yubis") lewat rapat terbatas, lalu TK dipindahkan ke kode perusahaan yang bertahan; perlakuannya kasus per kasus [P 01:06:06–01:08:42]. Volumenya disebut ~100-an juta ID perorangan, jauh lebih banyak dari perusahaan [P 01:09:16–01:09:30].
- (b) Hal ini **mendukung kebutuhan MDM dengan histori dan aturan survivorship**. Ada kekhawatiran **double kepesertaan → double klaim** [P 01:11:20–01:11:40].
- (a) **MDM sedang dibangun oleh tim BPJS** ("MDMG"/MDM, golden record). Hanya golden record yang akan dibagikan, ditampung di **wadah/penampung baru yang bisa di-hit dari luar** [P 01:25:45–01:26:50]. Desain itu masih dari tim BPJS sendiri.
- (c) **Permintaan eksplisit: tim BPJS meminta usulan desain yang lebih baik dari kita ("pak Iqlas")**, terutama data apa saja yang masuk ke golden copy/data hub dan bentuknya [P 01:26:52–01:27:29].
- (a) Menurut BPJS, MDM berada setelah staging dan sebelum DWH, sebagai bagian dari DWH; data harus bersih (DQ) dulu sebelum masuk MDM [S 00:09:18–00:09:42; P 00:24:07–00:24:19].
- (d) Kita meminta penjelasan **pembentukan kode TK dan kode kepesertaan** ("di-generate di mana, dari sistem apa") [P 00:35:24–00:35:52].
- (d) Kita menanyakan apakah agen PERISAI memiliki kode wilayah atau master tersendiri [P 00:40:21–00:40:42] [jawaban tidak jelas].
- (d) Kita menanyakan apakah golden record hanya berisi dimensi atau juga fact table [P 01:32:39–01:33:05]. Jawaban BPJS: masih konsep, mereka butuh masukan [P 01:33:37–01:33:45].
- (d) Kita bertanya apakah **discovery data master/dimensi baru** berjalan otomatis atau manual (mis. nilai drop-down yang di-hardcode, bukan di database) [S 00:25:26–00:26:22].

## 2. Integrasi (upstream / downstream)
- (a) **Replikasi sumber utama (Oracle, core system termasuk aplikasi mobile) ke big data**: memakai **Qlik (Replicate) CDC** yang membaca log/WAL Oracle, juga *initial load* [S 00:01:56–00:03:27; S 00:05:43–00:06:01]. Semua perubahan (insert/update; delete juga ditangkap) [S 00:06:37–00:07:23]. Data flow/ETL memakai **Talend** [S 00:02:16–00:02:27; S 00:31:20–00:31:24].
- (a) **Riwayat frekuensi**: dulu *initial load* sehari sekali (era tools sebelumnya [tidak jelas, "pakepo"]), lalu tiap 6 jam (360 menit), dan **sekarang paling cepat 1 jam sekali** ke Cloudera [S 00:03:54–00:04:21; S 00:08:06–00:08:24; S 00:29:51–00:29:57]. Tools lama dianggap "tidak bisa memenuhi kebutuhan" [S 00:07:56–00:08:04].
- (a) Ada juga batch, tetapi hanya sebagian. Talend disebut kurang optimal ("masih parah") untuk sebagian kasus [P 01:39:29–01:39:47] [sebagian tidak jelas].
- (b) Fakta ini **mengonfirmasi** asumsi stack (Qlik Replicate + Talend + CDP) dan pola upstream berbasis CDC.
- (a) **Streaming/real-time belum ada**. Ada keinginan ke arah sana, tetapi BPJS menilai streaming sulit untuk analitik (pembentukan dimensi/SCD) [S 00:04:21–00:04:47; S 00:32:02–00:32:18].
- (c) Sikap stakeholder: streaming **baru diadopsi bila ada kebutuhan bisnis nyata**, dalam horizon kajian 3 tahun ke depan [S 00:32:22–00:33:31].
- (a) **Pertukaran data eksternal**: rutin dengan **Kemenkes dan BPJS Kesehatan**, berupa data detail dan rekap [P 01:29:29–01:29:43]. Untuk **JKP** ada pertukaran (kemungkinan dengan Kemnaker) [tidak pasti] via **SFTP** yang dikelola tim operasional, dengan enkripsi **AES-256** dan *key exchange* masing-masing [P 01:41:27–01:43:49]. Ada pertukaran satu arah dan ada yang real-time "saling hit" API [P 01:45:53–01:46:05; P 01:50:49–01:51:07]. Validasi data masih **semi-otomatis** (ada orang di baliknya) [P 01:50:13–01:50:27].
- (a) **Kendala integrasi JKP/eksternal**: tipe data berbeda dan logika kepesertaan berbeda. BPJS TK menghitung **per tanggal**, sedangkan mitra **per bulan**, sehingga status aktif/nonaktif bisa berbeda. Mitra tidak mau mengubah logikanya [P 01:53:13–01:54:03].
- (a) **Kerja sama/inisiasi sharing data dengan mitra (mis. BPJS Kesehatan) belum terinisiasi formal**; tiap pihak menyiapkan ekosistem sendiri [P 01:29:05–01:29:27]. Ada rencana sharing data BPJS TK–BPJS Kesehatan yang disampaikan pimpinan [P 01:24:15–01:24:25] [sebagian tidak jelas].
- (c) Kita merekomendasikan transfer yang lebih aman (MFT, bukan SFTP polos) dan meminta catatan protokol yang dipakai sekarang [P 01:42:01–01:44:21].
- (a) **API Gateway**: gateway API ada di bawah tim **Infra**. Sebagian microservice sudah memakai API gateway, sebagian belum [S 00:11:47–00:12:25; S 00:38:41–00:38:51].
- (c) Arahnya **"satu pintu" lewat API Gateway**: data team cukup membuat endpoint, sedangkan autentikasi diatur API Gateway [S 00:12:01–00:12:58].
- (a) Saat ini **unit bisnis bisa langsung meminta data ke pihak eksternal/internal** tanpa satu pintu konsolidasi, sehingga permintaan berulang ke instansi yang sama (DGP/DJP? [tidak pasti]) [S 00:13:22–00:16:35]. Kita mengusulkan konsolidasi agar diminta sekali lalu dipakai banyak unit (HR, finance, sales).
- (d) Kita menanyakan apakah masih ada ingest eksternal berbentuk file/Excel atau semuanya sudah otomatis [P 01:31:11–01:31:31].

## 3. Arsitektur Data (layer ESDP di Cloudera)
- (a) **Penyimpanan di Cloudera**: tabel replikasi CDC masuk ke **Kudu**, yang bisa update/insert dan diperbarui per jam. Dari Kudu dibentuk tabel **Parquet** (diperbarui ≥1–2 jam) untuk query analitik kompleks yang lebih cepat [S 00:17:55–00:19:21; S 00:29:19–00:30:41]. Storage juga **HDFS** ("HDS") [S 00:02:33–00:02:48].
- (a) **Data mart** sebagian besar masih di Cloudera dalam bentuk Kudu/Parquet, hasil job ETL (Talend) [S 00:31:16–00:31:48].
- (a) Ada juga **Oracle** untuk kebutuhan tertentu (mis. finance/anggaran), dan disebut GoldenGate [tidak jelas] [S 00:45:44–00:47:04].
- (b) Kondisi saat ini adalah raw/staging (bronze), model, dan data bersih **berada di tempat yang sama, hanya dibedakan skema** [S 00:44:54–00:45:06]. Ini mengonfirmasi bahwa layering yang tegas (Opsi B) belum ada.
- (b) BPJS **menerima pendekatan medallion** ("bagus untuk kita terapkan"), walau selama ini jarang memakai istilah bronze/silver/gold [S 00:44:12–00:44:48].
- (a) **DRC**: Cloudera di DRC **belum ada komponennya**. Hanya ada tempat/rack di DRC, dan sempat ada uji coba 1 mesin. Lisensi Cloudera dibayar per tahun. Hubungan DC–DRC (active-active/multi-site) belum didefinisikan dan arsitektur infra belum dibentuk [S 00:35:03–00:37:39] [sebagian tidak jelas].
- (a) Zona jaringan (**DMZ / green zone**) belum terdefinisi jelas. Perlu menyamakan persepsi tentang letak API Gateway (router → firewall → DMZ, lalu NAT ke dalam) [S 00:38:53–00:40:30].
- (d) Kita bertanya di mana **"pusat data"/data hub** sebaiknya berada: di dalam environment big data atau di luar (public-facing). Bila di luar, perlu pipeline sendiri [S 00:20:13–00:21:13; S 00:33:35–00:34:47; P 01:25:01–01:25:11].
- (c) Ekspektasi stakeholder: kajian melihat **3 tahun ke depan**, memaksimalkan teknologi yang ada, dan menjelaskan **apa yang harus diubah, kapan, dan mengapa** [S 00:32:26–00:32:36; S 00:47:28–00:47:46; P 00:28:18–00:28:31].
- (d) Kita bertanya apakah ada **redundansi Kudu vs Talend vs tools lain**, dan apakah perlu migrasi [P 00:28:43–00:28:55].

## 4. Pemodelan Data
- (a) **SCD** dipakai untuk dimensi (tipe 1, 2, 3 disebut). Tabel referensi diubah menjadi dimensi lewat ETL [S 00:00:37–00:00:49; S 00:19:34–00:19:50].
- (a) Contoh **dimensi lokasi**: kantor wilayah → cabang [S 00:05:07–00:05:21].
- (a) Klasifikasi master/transaksi/referensi menurut BPJS: **master** berisi data peserta; **transaksi** berisi penerimaan iuran dan pembayaran klaim; **referensi** berisi kode wilayah/kota (bisa berubah karena pemekaran) [S 00:26:42–00:28:43].
- (c) BPJS menyatakan arsitektur data mereka "**agak beda**" dan perlu dipetakan mana yang master/transaksi/referensi [S 00:26:22–00:27:09].
- (a) **Model data kepesertaan pernah dibuat ~3 tahun lalu** (asesmen data model, dibantu pihak lain) dan tidak banyak berubah. Model itu belum mencakup **JKP** (program baru) [P 01:21:33–01:22:26]. Ada "data block" per deputi [P 01:23:16–01:23:26].
- (a) **Segmen peserta**: BPU, PU, PMI, Jakon [P 01:01:10–01:01:16]. Data peserta dikategorikan primer vs sekunder. Data **primer dikunci** dan perubahannya harus datang ke kantor cabang, sedangkan data **sekunder bisa diubah lewat aplikasi** [P 01:01:22–01:02:28].
- (a) **Status TK**: *TK aktif* (masih membayar iuran), *TK nonaktif* (tidak membayar lagi tetapi masih punya saldo), *TK keluar* (saldo sudah diambil) [P 01:55:30–01:55:52]. Untuk **PU yang perusahaannya belum membayar, TK tetap aktif** dan iurannya dicatat sebagai **piutang**; JKK tetap menjamin [P 01:54:33–01:55:05; P 01:56:30–01:56:36].
- (a) Ada **5 program**. JKP disebut "seperti program bonus": tidak ada tambahan iuran dari perusahaan, pemerintah yang menambah, dan eligibility terkait JKN/BPJS Kesehatan [P 01:46:17–01:48:33] [sebagian tidak jelas].
- (b) Fakta ini mendukung shared dims Peserta, Pemberi Kerja, Kantor, dan Program, serta kebutuhan relasi peserta–program (many-to-many) yang kita angkat [P 00:30:09–00:30:35].
- (c) **Arahan untuk scope investasi: jangan masuk terlalu dalam** ("punya dunianya sendiri"). Cukup ketahui domain itu mencakup data apa dan siapa pemiliknya, lihat dari helicopter view, dan fokus pada efektivitas sharing data internal [P 00:41:58–00:45:56]. **PERISAI** (aplikasi agen, di bawah building block kepesertaan) juga tidak perlu didalami sampai level fee [P 01:19:49–01:20:29].
- (a) **Saldo/iuran berada di domain kepesertaan** [P 01:21:11–01:21:17].
- (d) Kita bertanya tipe SCD rata-rata yang dipakai (tipe 2?) untuk kebutuhan historis/forecasting [P 01:31:33–01:31:49], dan tools pembentuk dimensi vs fact (Kafka? Talend?) [P 01:34:25–01:35:19] [jawaban tidak jelas].
- (c) Kebutuhan gold yang disebut: forecasting, risk management (**risiko likuiditas**: maturity iuran/klaim untuk tim investasi/treasury), dashboard (star schema), dan reporting relasional [S 00:43:04–00:45:46; P 00:42:52–00:43:31].

## 5. Metadata & Kualitas Data
- (a) **Dataedo** dipakai sekitar 3–4 tahun. Lisensi enterprise dibeli langsung tanpa partner, setelah uji coba 30 hari [P 00:21:08–00:21:14; P 01:13:24–01:13:38]. Sifatnya pasif: harvesting tabel dan prosedur dari Oracle, deskripsi/ERD dilengkapi manual di web [P 00:21:22–00:22:16].
- (a) Isinya saat ini terutama **metadata teknis** (tabel fisik, view, prosedur Oracle), nullable, dan sebagian deskripsi. Business metadata baru sebagian [P 01:13:58–01:14:44; P 01:17:31–01:18:05].
- (a) Tim yang mengelola data/Dataedo berada di bawah "**Mudina**" [tidak jelas] yang baru [P 01:16:53–01:17:05].
- (b) Tim lain sebelumnya pernah menawarkan pengganti Dataedo, tetapi BPJS merasa **Dataedo masih cukup**. Belum ada arah untuk mengganti; yang mungkin dilakukan adalah ekspansi/optimasi [P 00:23:13–00:23:55]. Ini **konsisten** dengan asumsi federated catalog (Atlas + Dataedo).
- (c) Review Dataedo boleh dilakukan, tetapi jangan sampai melebar dari waktu proyek; fokusnya 3 tahun ke depan [P 00:22:22–00:22:52].
- (a) **Data lineage**: BPJS menyatakan "**belum punya**" (ditandai merah). Data catalog belum dimanfaatkan, policy belum ada [P 01:36:23–01:36:41].
- (a) **DQ**: kualitas data diukur dan ada "rumus kualitas data" untuk data peserta [P 01:01:36–01:01:44]. Rekonsiliasi berjalan harian [tidak pasti] [P 01:01:54–01:02:02].
- (c) Usulan kita: data discovery → profiling → katalog/metadata → klasifikasi → kebijakan, serta lineage (graph database disebut sebagai opsi) [P 00:09:49–00:11:13; P 00:19:01–00:19:52].
- (d) Kita menanyakan sejauh mana isi Dataedo/metadata sekarang dan proporsi technical vs business metadata [P 01:12:20–01:14:12].

## 6. Keamanan & Privasi
- (c) Prioritas saat ini adalah **kepatuhan UU PDP** untuk pengungkapan data [P 00:08:50–00:09:01].
- (c) Konsep yang kita bawa: **data minimization**, contohnya kolom **agama** (perlu diambil atau tidak) [P 00:11:30–00:12:51]; **DLP**, masking, dan consent pemilik data [P 00:09:09–00:09:37; P 00:08:31–00:08:45]; contoh kolom sensitif NIK/KTP, KK, nama ibu kandung, nomor telepon [P 00:10:31–00:10:48].
- (a) Saat ini **masking diterapkan di sebagian titik** [P 00:09:26–00:09:29] [tidak jelas].
- (b) Kebijakan berbasis klasifikasi (retensi aktif → frozen, pemusnahan media sesuai standar NIST) **belum ada** [P 01:36:45–01:38:06; P 01:38:39–01:38:45]. Ini mendukung pendekatan classification-based controls.
- (a) Pertukaran eksternal memakai AES-256 dan key exchange (lihat §2).

## 7. Tata Kelola
- (a) **SOP/standar tata kelola untuk pertukaran data (perjanjian kerja sama) belum ada**. Perjanjian kerja sama bisa diajukan siapa pun, dan setelah ada kesepakatan baru masuk ke area teknis (pembatasan akses, kuota per hari) [S 00:13:48–00:15:07]. Operasional dijalankan unit lain [S 00:15:13–00:15:25] [tidak jelas].
- (a) Ada **Perdir tata kelola data**, yang dirujuk terkait prosedur [P 01:18:23–01:18:29] [tidak pasti].
- (a) Disebut "**4 pilar**" di bawah [tidak jelas, "pama-dek"], termasuk integrasi dengan MDM. Pembahasan data governance selama ini "tidak pernah masuk" [P 02:04:27–02:05:01].
- (c) Kepemilikan/ownership data per domain perlu diketahui agar consent dan pengungkapan bisa diatur [P 00:08:33–00:08:42; P 00:31:50–00:31:57; P 00:44:22–00:44:29].
- (d) **Action**: hadirkan **tim data governance** BPJS pada pertemuan minggu depan [P 02:09:11–02:09:31].

## 8. Penyimpanan + Dokumen & Konten
- (a) Teknologi penyimpanan: HDFS, Kudu, Parquet (lihat §3).
- (a) Laporan iuran dari perusahaan kadang berupa **file Excel** yang lalu diproses tim [P 00:05:38–00:05:49].
- (d) Kita menanyakan isu media lama dan retensi jangka panjang: data peserta bisa ~40+ tahun (masuk umur 19, pensiun ~56–60) [P 00:39:35–00:39:49]. Kebijakan retensi/lifecycle perlu dirumuskan.
- Tidak ditemukan pembahasan spesifik tentang dokumen/konten (ECM) di rekaman [tidak ditemukan].

## 9. DW & BI
- (a) Pengguna analitik yang query-nya kompleks diarahkan ke tabel Parquet; data mart dibentuk sesuai kebutuhan bisnis [S 00:30:25–00:31:00].
- (a) Ada data yang langsung dikonsumsi **core system** dari environment big data, tetapi tidak banyak (mis. report) [S 00:21:48–00:22:04]. Core system mencakup aplikasi layanan peserta, klaim, dsb. [S 00:22:24–00:22:42].
- (a) Ada **banyak cube** untuk aplikasi finance [S 00:21:36–00:21:46].
- (a) Kebutuhan gold: forecasting, risk management/likuiditas, dashboard, reporting (lihat §4). Dimensi waktu/tanggal penting untuk forecasting [P 01:32:23–01:32:35].
- (d) Kita bertanya volume DWH/kapasitas. BPJS menjawab perlu, tetapi fokus pada data dulu [P 01:30:27–01:30:45].

## Roadmap / Admin
- (a) **Proyek berjalan 6 minggu, sudah memasuki minggu ke-2** [P 00:26:15–00:26:20; P 02:08:55–02:08:59]. Pernah disebut ±2 minggu untuk bagian data [P 00:26:08].
- (c) Ekspektasi stakeholder: buat **timeline/agenda yang jelas**: hari dan minggu kapan bertemu unit mana, supaya unit bisa menyiapkan memo/izin atasan [S 00:41:12–00:41:52]. Pastikan **scope** BPJS jelas [S 00:41:54–00:42:00].
- (c) Konsultan diminta banyak berdiskusi dengan unit lain: perencanaan TI, operasional IT, pengembangan, riset, solution architect [S 00:47:56–00:48:22]. Online lebih efektif bila memungkinkan [S 00:49:02–00:49:20].
- (d) **Action (BPJS)**: share dokumen yang dibutuhkan ke **Google Drive**; kekurangan disebut ulang [S 00:48:48–00:49:30]. Dokumen data flow kepesertaan dan diagram (masih **draft**) akan dibagikan [P 01:23:35–01:23:41; P 01:40:45–01:41:07]. Hasil kajian konsultan sebelumnya (disebut "2013–2023" [tidak pasti]) juga disebut [P 02:08:09–02:08:15].
- (d) **Action (kita)**: minta feedback dan komitmen next step; pertemuan berikutnya minggu depan bersama tim data governance [P 02:08:53–02:09:37].
- (c) Ada kritik internal bahwa minggu-minggu awal belum banyak progres karena data belum tersedia; "kalau tidak dikawal dari depan" berisiko [S 00:51:36–00:52:20] [sebagian tidak jelas].

## Lainnya
- (a) Pada implementasi Cloudera sebelumnya (tim data dipimpin Pak Widi), Qlik disebut "lambat banget" saat harus dipakai [P 02:05:09–02:05:23] [tidak jelas].
- (a) Alur iuran: perusahaan melapor → bank → mapping dana → saldo; hasil investasi dibagikan tahunan berdasarkan portofolio; saldo berkurang karena klaim [P 00:04:20–00:06:05]. Ini dijelaskan oleh pihak kita sebagai pemahaman, belum dikonfirmasi BPJS.
- Bagian non-substantif: makan siang/obrolan pribadi [P 01:58:00–02:03:10; P 02:05:59–02:07:39; P 02:09:59–akhir], koordinasi tamu/lobby [P 01:52:37–01:53:01], screen-share hening/halusinasi ASR [P 00:46:00–00:56:36; P 00:56:36–01:00:55 tanpa ucapan].
