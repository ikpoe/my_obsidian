# 1.2 Maksud, Tujuan, dan Ruang Lingkup

## 1.2.1 Maksud

Subbab 1.1 menunjukkan bahwa BPJS Ketenagakerjaan telah memiliki kebijakan tata kelola data yang lengkap, platform data yang sudah beroperasi, dan sejumlah inisiatif yang sedang berjalan. Namun ketiganya belum terhubung dalam satu rancangan. Tanpa rancangan bersama, setiap inisiatif berisiko menetapkan definisi, kendali, dan jalur pertukaran datanya sendiri. Akibatnya, fragmentasi yang ingin diatasi justru dapat terulang.

Blueprint ini disusun sebagai **acuan arsitektur data bersama** yang menerjemahkan Peraturan Direksi Nomor 4/PERDIR.06/042025 ke dalam rancangan yang dapat dilaksanakan pada wilayah ESDP. Dokumen ini menjadi pegangan Komite Pengarah TI dan forum tata kelola data dalam menetapkan prioritas. Bagi Unit Manajemen Data dan unit teknologi informasi, dokumen ini menjadi rujukan saat merancang dan menilai setiap perubahan. Bagi unit bisnis pemilik data, dokumen ini memperjelas peran yang diharapkan dari mereka (D1 hlm. 7–10). Blueprint juga menjadi dasar komunikasi dengan mitra kementerian/lembaga mengenai cara BPJS Ketenagakerjaan menerima dan menyalurkan data secara aman dan terstandar (D1 hlm. 53–58).

## 1.2.2 Tujuan

Tujuan Blueprint diturunkan dari tantangan yang ditemukan pada asesmen kondisi saat ini dan diselaraskan dengan tiga tema peta jalan. Tema Tahun 1 adalah Fondasi dan Kendali, Tahun 2 Integrasi dan Pertukaran Terstandar, dan Tahun 3 Pemanfaatan dan Skala. Setiap tujuan dirumuskan sedemikian rupa sehingga kemajuannya dapat diamati. Ukuran keberhasilan rinci diuraikan pada subbab 3.5.

1. **T1 – Akuntabilitas data yang jelas.** Setiap domain data prioritas memiliki pemilik dan pengelola (*steward*) yang ditetapkan, bekerja dalam forum dan mekanisme keputusan yang berjalan. Tujuan ini menjawab belum jelasnya kewenangan atas perubahan elemen data (D2 hlm. 26) dan belum didefinisikannya fungsi pengelola master data (D2 hlm. 24).
2. **T2 – Data terlindungi sesuai klasifikasinya.** Setiap dataset di ESDP memiliki klasifikasi, aturan akses, serta jadwal retensi dan pemusnahan yang selaras dengan UU 27/2022 dan PP 33/2026 [perlu konfirmasi tanggal berlaku].
3. **T3 – Platform yang andal dan dapat dipulihkan.** ESDP memiliki pemisahan zona yang tegas, rencana kapasitas, dan kemampuan pemulihan yang telah diuji. Tujuan ini menjawab risiko penempatan *data lake* dan *data warehouse* dalam satu lingkungan (D2 hlm. 21) serta status pusat data cadangan yang masih perlu dipastikan (D2 hlm. 50) [perlu konfirmasi].
4. **T4 – Pertukaran data terstandar melalui satu pintu.** Seluruh pertukaran data dengan mitra eksternal berjalan melalui Data Hub dan dilandasi perjanjian kerja sama, dokumen teknis, serta kontrak data yang terdaftar. Kondisi ini menggantikan platform yang beragam sesuai mitra (D2 hlm. 22, 40).
5. **T5 – Satu identitas dan satu definisi.** Entitas inti, yaitu peserta, pemberi kerja, dan program, memiliki *golden record* dan definisi baku. Kualitas dan *freshness*-nya dipantau pada titik kritis aliran data (D2 hlm. 23–28).
6. **T6 – Satu angka untuk pengambilan keputusan.** Indikator kinerja utama dihitung dari lapisan metrik bersama di atas data tersertifikasi, sehingga dasbor dan laporan memberikan angka yang sama (D2 hlm. 22).

Keterkaitan setiap tujuan dengan tema tahunan dan area arsitektur ditunjukkan pada Gambar 1.5. Tujuan T1 sampai T3 menjadi fokus utama Tahun 1 karena menjadi prasyarat bagi tujuan lainnya. Integrasi dan pemanfaatan data yang dibangun sebelum kepemilikan dan klasifikasinya jelas justru memperbesar risiko.

![[G1-5-Tujuan-Tema-Area.png]]
*Gambar 1.5 Keterkaitan Tujuan Blueprint dengan Tema Peta Jalan dan Area Arsitektur*

## 1.2.3 Ruang Lingkup

Ruang lingkup Blueprint ditetapkan dengan menempatkan ESDP sebagai poros. Pembatasan ini dipilih secara sadar. ESDP adalah titik tempat data dari berbagai sistem sumber bertemu, sehingga di titik inilah standar, kendali, dan kualitas dapat diterapkan secara konsisten tanpa harus mengubah setiap aplikasi sumber. Integrasi dipandang dari dua arah. Arah hulu (*upstream*) mencakup aliran data dari sistem sumber ke ESDP. Arah hilir (*downstream*) mencakup penyaluran data dari ESDP kepada pengguna internal dan mitra eksternal (Gambar 1.4).

![[G1-4-Ruang-Lingkup.png]]
*Gambar 1.4 Ruang Lingkup Blueprint dengan ESDP sebagai Poros*

Di dalam lingkup tersebut, Blueprint mencakup sepuluh area arsitektur yang diuraikan pada BAB IV. Kesepuluh area itu adalah tata kelola data; arsitektur platform data ESDP; pemodelan data; penyimpanan, retensi, dan resiliensi; integrasi dan interoperabilitas; manajemen data induk; metadata dan kualitas data; keamanan dan pelindungan data; *data warehouse*, BI, dan lapisan metrik; serta parameter yang perlu ditetapkan. Pembagian ini sejalan dengan struktur pengaturan dalam Perdir 4/2025, yang meliputi arsitektur, pemodelan, metadata, kualitas, *data warehouse* dan big data, master data, penyimpanan, keamanan informasi, serta kolaborasi data (D1 hlm. 14–17, 40–58, 64–92). Ketiga *use case* prioritas dalam kerangka acuan kerja juga terwadahi di dalamnya. Ketiganya adalah kesiapan infrastruktur dan platform ESDP, pertukaran data melalui API, dan tata kelola data. Rinciannya diuraikan pada subbab 3.3.

Batas lingkup ditegaskan sebagai berikut. SMILE sebagai sistem inti, aplikasi pendukung, dan *Enterprise Content Management* (ECM) berada di luar platform. Penyimpanan internal dan perubahan pada sistem-sistem tersebut tidak dirancang dalam Blueprint ini. Sistem-sistem tersebut hanya disebut sebagai antarmuka, yaitu sumber atau tujuan data beserta kontrak data yang mengatur pertukarannya. Hubungan dengan sistem di luar platform berlangsung melalui jalur integrasi yang terkelola, yaitu API dan SFTP, dengan replikasi terkelola untuk penyerapan data internal [perlu konfirmasi]. Koneksi langsung ke basis data mitra tidak menjadi bagian dari rancangan target.

Periode perencanaan Blueprint ini adalah tiga tahun, dengan jadwal yang dirinci per kuartal dari Tahun 1 Kuartal 1 hingga Tahun 3 Kuartal 4. Tahun 1 diasumsikan dimulai pada 2027 [perlu konfirmasi] dan diselaraskan dengan siklus RKAT. Beberapa hal berada di luar lingkup dokumen ini, yaitu:

- rincian rancangan teknis implementasi;
- spesifikasi pengadaan dan anggaran;
- pemilihan produk atau alat; serta
- penetapan nilai parameter.

Untuk parameter, Blueprint menetapkan *apa* yang harus diputuskan beserta pemilik keputusannya. Penetapan nilainya, misalnya ambang kualitas, target *freshness*, dan masa retensi, menjadi keputusan forum tata kelola dalam pelaksanaan peta jalan (subbab 4.10).

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Catatan aset:** sumber gambar yang dapat disunting tersedia di `assets/` (`G1-4-Ruang-Lingkup.svg` dan `G1-5-Tujuan-Tema-Area.svg`; teks asli, elemen dikelompokkan dalam `<g>` ber-id). PNG diekspor dari SVG pada 220 dpi.

**Keputusan yang perlu diambil ikpu**
1. Penomoran sub-subbab 1.2.1–1.2.3. Pilihannya mempertahankan penomoran ini atau menggabungkan isinya menjadi narasi tanpa sub-judul seperti pada 1.1.
2. Penetapan enam tujuan (T1–T6) sebagai rumusan resmi. Bila disetujui, kode T1–T6 akan dipakai konsisten di 3.1 dan 3.5.
3. Penekanan "fokus utama/lanjutan" pada Gambar 1.5 bersifat indikatif. Perlu diputuskan apakah tetap ditampilkan atau cukup memetakan tujuan ke area.
4. Rumusan batas integrasi. Penyerapan internal saat ini memakai replikasi/CDC dan ETL (D2 hlm. 21). Perlu dipastikan "API/SFTP" berlaku untuk pertukaran dengan pihak di luar platform saja, sementara replikasi terkelola tetap diizinkan untuk hulu internal.
5. Pernyataan bahwa koneksi langsung ke basis data mitra tidak masuk rancangan target. Sumbernya temuan integrasi Kemnaker, yang dirujuk tanpa rincian teknis. Perlu diputuskan apakah kalimat ini cukup aman untuk dibawa ke klien.

**Konfirmasi ke klien**
1. Tahun 1 = 2027 dan penyelarasan dengan siklus RKAT.
2. Rumusan resmi tiga *use case* prioritas sesuai kerangka acuan kerja, untuk dikutip di 3.3.
3. Status pusat data cadangan (D2 hlm. 50 bertentangan dengan temuan wawancara).
4. Posisi ECM: hanya antarmuka, atau ada metadata dokumen yang perlu masuk ke katalog ESDP.
5. Tanggal berlaku PP 33/2026.
