# 1.2 Maksud, Tujuan, dan Ruang Lingkup

## 1.2.1 Maksud

Kegiatan ini dimaksudkan untuk meninjau arsitektur interoperabilitas data ketenagakerjaan yang sedang berjalan dan merancang arsitektur yang dituju. Hasil reviu dan rancangan tersebut menjadi dasar pengembangan interoperabilitas data yang terstruktur, aman, dan berkelanjutan.

Hasilnya dituangkan dalam Blueprint Enterprise Service Data Platform (ESDP) ini. Blueprint menggambarkan kondisi saat ini, kebutuhan target, prinsip dan pola interoperabilitas, arsitektur target, serta tahapan implementasinya, sehingga manajemen dapat melihat arsitektur data enterprise secara utuh dan menyeluruh. Blueprint ini menjadi:

- pegangan Komite Pengarah TI dan forum tata kelola data dalam menetapkan prioritas;
- rujukan Unit Manajemen Data dan unit teknologi informasi dalam merancang dan menilai setiap perubahan;
- penjelas peran unit bisnis pemilik data (D1 hlm. 7–10); serta
- rujukan dalam menyusun standar dan perjanjian pertukaran data dengan mitra.

## 1.2.2 Tujuan

Tujuan dokumen ini adalah:

1. **Memetakan kondisi saat ini dan kesenjangannya.** Dokumen ini memotret arsitektur, lanskap data, integrasi aplikasi, dan pertukaran data yang sedang berjalan, termasuk dengan kementerian, lembaga, dan mitra eksternal. Dari gambaran itu dikenali kesenjangan pada aspek data, tata kelola, keamanan, infrastruktur, dan teknologi yang perlu ditutup agar interoperabilitas dapat berkembang.

2. **Merancang arsitektur data dan arsitektur ESDP yang dituju.** Rancangan ini menjelaskan komponen, hubungan, aliran data, dan mekanisme integrasinya. Rancangan ini juga menegaskan peran dan batas fungsi setiap komponen, yaitu sistem sumber (*System of Record*), *Master Data Management*, ESDP, Cloudera, konsumen data, dan pihak eksternal, sehingga tidak ada fungsi yang tumpang tindih atau tidak dimiliki siapa pun.

3. **Merumuskan kerangka interoperabilitas data dan mengujinya pada kasus nyata.** Kerangka ini mencakup pola integrasi, mekanisme pertukaran dan API, kontrak dan skema data, keamanan, tingkat layanan, pemantauan, rekonsiliasi, serta proses *onboarding* mitra. Kerangka tersebut diuji melalui tiga *use case*, yaitu infrastruktur dan kesiapan platform ESDP, API dan pertukaran data, serta tata kelola data, masing-masing dengan pola arsitektur *end-to-end*.

4. **Menyusun tahapan implementasi tiga tahun.** Tahapan ini menggambarkan arsitektur antara dari kondisi saat ini menuju kondisi target, urutan prioritas, ketergantungan antarlangkah, serta target kematangan kapabilitas di setiap tahap, sehingga pengembangan dapat dijalankan bertahap tanpa mengganggu layanan yang sedang berjalan.

5. **Memberikan rekomendasi teknologi dan arah pengembangan berikutnya.** Rekomendasi ini mencakup kapabilitas teknologi yang dibutuhkan, faktor pendorong investasi utama, dan tahap lanjutan pengembangan ESDP, sebagai bahan pertimbangan manajemen dalam perencanaan dan penganggaran.

## 1.2.3 Ruang Lingkup

Blueprint ini menempatkan ESDP sebagai pusat interoperabilitas data, tempat standar, kendali, kualitas, dan tata kelola pertukaran diterapkan secara konsisten. Cakupannya meliputi arah hulu, yaitu aliran data dari sistem sumber ke ESDP, dan arah hilir, yaitu penyaluran data kepada pengguna internal dan mitra eksternal. Kebutuhan *real-time* dapat tetap dilayani langsung dari sistem inti di bawah tata kelola yang sama (Gambar 1.4).

![[G1-4-Ruang-Lingkup.png]]
*Gambar 1.4 Ruang Lingkup Blueprint dengan ESDP sebagai Pusat*

**Interoperabilitas data menjadi inti Blueprint**, mulai dari prinsip dan pola integrasi, standar API dan format pertukaran, Data Hub, hingga tata kelola pertukaran dengan mitra. Area lain dirancang sebagai penopangnya, yaitu tata kelola data, data induk, metadata dan kualitas data, platform ESDP, serta *data warehouse* dan BI. Tiga hal dibahas lintas area:

- **tata kelola data dan pertukaran**, termasuk pemilik data, forum keputusan, dan aturan berbagi data;
- **keamanan dan pelindungan data pribadi**, sesuai UU 27/2022; serta
- **integrasi lintas instansi** dengan kementerian, lembaga, dan mitra eksternal lainnya.

Periode perencanaan Blueprint adalah tiga tahun, dengan Tahun 1 diasumsikan dimulai pada 2027 [perlu konfirmasi].

Hal-hal berikut berada di luar lingkup:

- **Sistem sumber dan aplikasi pendukung.** Sistem ini tidak dirancang ulang dan hanya dipandang sebagai sumber atau tujuan data.
- **Implementasi.** Rancangan teknis rinci, pengujian, dan *deployment* tidak termasuk.
- **Pengadaan.** Spesifikasi, anggaran, dan pemilihan produk tidak termasuk.
- **Nilai parameter operasional**, seperti RPO/RTO, masa retensi, dan ambang kualitas. Blueprint hanya menetapkan apa yang perlu diputuskan dan siapa pemilik keputusannya.

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Reviu ikpu (2026-11-06, 1.2.3 ringkas):** 1.2.3 dipadatkan. Rincian sistem di luar lingkup (SMILE, ECM, jalur integrasi), keselarasan dengan Perdir 4/2025, uraian *use case* (sudah di 1.2.2), dan rujukan D6 dihapus; hal di luar lingkup cukup dikelompokkan.

> **Reviu ikpu (2026-11-06, 1.2.3):** nama mitra pada butir integrasi lintas instansi diganti sebutan umum (kementerian, lembaga, dan mitra eksternal), selaras dengan 1.1. Pengujian dan *deployment* ditambahkan ke daftar di luar lingkup sesuai proposal. "Pelaksanaan peta jalan" diganti "saat Blueprint dijalankan". Perbaikan format paragraf setelah butir.

> **Reviu ikpu (2026-11-06):** 1.2.2 dielaborasi: setiap tujuan diberi judul singkat dan penjelasan satu sampai dua kalimat, tetap mengikuti lima tujuan TOR hlm. 4 dan cakupan TOR hlm. 5–6 dengan bahasa sendiri.

> **Reviu ikpu (2026-11-06):** 1.2.1 Maksud ditulis ulang agar menyambung dengan 1.1 (reviu arsitektur interoperabilitas yang berjalan sebagai dasar rancangan, pandangan arsitektur data enterprise yang utuh), mengikuti maksud TOR hlm. 4 dengan bahasa sendiri.

> **Reviu internal (2026-11-06):** (C) ruang lingkup diperkaya dengan tata kelola, keamanan dan pelindungan data pribadi (kategori umum dan spesifik UU PDP), serta integrasi lintas instansi. "Titik pusat integrasi" diganti "pusat interoperabilitas" dan ditambah kalimat bahwa *real-time* boleh langsung dari sistem inti (R2). ESB tidak dimasukkan. (D) inti interoperabilitas tidak diubah.

> **Penyelarasan TOR (2026-11-05, keputusan ikpu):** maksud dan tujuan ditulis langsung tanpa menyebut dasarnya; rujukan TOR dicatat di sini.
> - Maksud merangkum dua maksud TOR (TOR hlm. 4): hasil reviu kondisi eksisting dan rancangan arsitektur interoperabilitas, serta Blueprint ESDP sebagai acuan enterprise. Dasar Renstra 2027–2031 F3 dan F1 (TOR hlm. 3).
> - Tujuan 1–5 di 1.2.2 adalah tujuan TOR 1–5 (TOR hlm. 4) dengan rumusan sendiri, urutan tidak diubah. Urutan TOR mengikuti alur kerja (reviu → arsitektur → kerangka → peta jalan → rekomendasi), sehingga interoperabilitas ditonjolkan lewat isi butir 1 dan 3, bukan lewat urutan. Ruang lingkup terkait: tujuan 1 → RL 1–2; tujuan 2 → RL 3–4; tujuan 3 → RL 5–6; tujuan 4 → RL 7; tujuan 5 → RL 8 (TOR hlm. 5–6).
> - Daftar T1–T6 dan Gambar 1.5 (G1-5) dipindah ke 3.1 sebagai kondisi target K1–K6 dan Gambar 3.2 (G3-2). Pemetaan K1–K6 ke TOR dicatat di Catatan 3.1.

> **Pembaruan R2 (2026-11-05):** tujuan pertukaran data (sebelumnya T4 "melalui satu pintu") dirumuskan ulang menjadi satu tata kelola dengan jalur sesuai kelas kebutuhan dan dipindah menjadi T1 (kini K1 di 3.1), berdasarkan diskusi dengan BPJS Ketenagakerjaan pada 4 November 2026 (R2). Pemetaan tujuan ke area BAB IV kini ada di Gambar 3.2 (3.1).

> **Integrasi D6 (2026-11-02):** batas parameter kini menyebut bahwa ambang kualitas data peserta diselaraskan dengan skala indeks dalam D6 (hlm. 30). Nilai ambang sengaja tidak dikutip.

> **Catatan aset:** sumber gambar yang dapat disunting tersedia di `assets/` (`G1-4-Ruang-Lingkup.svg`; teks asli, elemen dikelompokkan dalam `<g>` ber-id, PNG diekspor pada 220 dpi). Berkas G1-5 dihapus; penggantinya `G3-2-Kondisi-Target-Tema-Area.excalidraw.md` di 3.1.

**Keputusan yang perlu diambil ikpu**
1. Maksud dan ruang lingkup kini menempatkan interoperabilitas sebagai inti dan area lain sebagai penopang. Judul di dalam Gambar 1.4 juga perlu dicek karena keterangannya sudah diganti menjadi "ESDP sebagai Pusat".
2. **Dipindah ke 3.1 (2026-11-05).** ~~Tujuan pertukaran data dipindah ke urutan pertama (T1) dan dirumuskan ulang sebagai satu tata kelola dengan jalur sesuai kelas kebutuhan. Tujuan lama T1–T3 menjadi T2–T4, dan Gambar 1.5 kini (berkas v3) mengurutkan baris menurut prioritas, T1 paling atas. Kolom tahun tetap menandai T1 sebagai fokus utama Tahun 2, dan paragraf 1.2.2 menjelaskan bahwa urutan prioritas tidak sama dengan urutan tahun.~~
3. **Selesai (2026-11-05):** `00-Admin/01-TOR.md` dilengkapi dari TOR asli dan maksud/tujuan diselaraskan. Ruang lingkup 1.2.3 belum menyebut batas peran SoR–MDM–ESDP–Cloudera, arsitektur transisi, dan rekomendasi teknologi; apakah perlu ditambah?
4. Penomoran sub-subbab 1.2.1–1.2.3. Pilihannya mempertahankan penomoran ini atau menggabungkan isinya menjadi narasi tanpa sub-judul seperti pada 1.1.
5. **Dipindah ke 3.1 (2026-11-05).** ~~Penetapan enam tujuan (T1–T6) sebagai rumusan resmi.~~
6. **Dipindah ke 3.1 (2026-11-05).** ~~Penekanan "fokus utama/lanjutan" pada gambar tujuan–tema–area.~~
7. Rumusan batas integrasi. Penyerapan internal saat ini memakai replikasi/CDC dan ETL (D2 hlm. 21). Perlu dipastikan "API/SFTP" berlaku untuk pertukaran dengan pihak di luar platform saja, sementara replikasi terkelola tetap diizinkan untuk hulu internal.
8. Pernyataan bahwa koneksi langsung ke basis data mitra tidak masuk rancangan target. Sumbernya informasi integrasi Kemnaker, yang dirujuk tanpa rincian teknis. Perlu diputuskan apakah kalimat ini cukup aman untuk dibawa ke klien.

**Konfirmasi ke klien**
1. Tahun 1 = 2027 dan penyelarasan dengan siklus RKAT.
2. **Selesai (2026-11-05):** rumusan tiga *use case* tersedia di TOR (hlm. 4–5). Objek data per *use case* masih perlu disepakati.
3. Status pusat data cadangan (D2 hlm. 50 bertentangan dengan informasi dari wawancara).
4. Posisi ECM: hanya antarmuka, atau ada metadata dokumen yang perlu masuk ke katalog ESDP.
5. Tanggal berlaku PP 33/2026.
