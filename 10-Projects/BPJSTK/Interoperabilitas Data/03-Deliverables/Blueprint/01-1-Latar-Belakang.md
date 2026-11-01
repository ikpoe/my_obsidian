# 1.1 Latar Belakang

BPJS Ketenagakerjaan mengemban amanat menyelenggarakan jaminan sosial bagi seluruh pekerja Indonesia. Amanat ini menempatkan data sebagai aset yang menentukan mutu layanan. Hak pekerja atas manfaat hanya dapat dipenuhi dengan tepat bila identitas peserta, riwayat iuran, dan status pemberi kerja tercatat dengan benar dan dapat dipercaya. Pada akhir 2025, hampir tujuh puluh juta tenaga kerja tercatat sebagai peserta. Tujuh dari sepuluh di antaranya berstatus aktif, berasal dari ratusan ribu pemberi kerja dan berbagai segmen pekerja, dengan dana investasi yang kini mendekati seribu triliun rupiah (Gambar 1.1). BPJS Ketenagakerjaan menargetkan kepesertaan 70 juta pekerja dan dana investasi Rp1.001 triliun pada 2026 (Laporan Terintegrasi 2025, hlm. 14).

![[G1-1-Kepesertaan.png]]
*Gambar 1.1 Kepesertaan dan Dana Investasi BPJS Ketenagakerjaan per 31 Desember 2025*

Dengan skala ini, ketidaktepatan data sekecil apa pun berdampak langsung pada jutaan pekerja dan pada akuntabilitas pengelolaan dana publik.

Data kepesertaan juga tidak lagi hanya dipakai untuk kepentingan internal. Kebijakan perlindungan sosial nasional makin bertumpu pada kemampuan lembaga untuk saling memadankan dan memanfaatkan data. Validasi identitas peserta bergantung pada data kependudukan Dukcapil (D2 hlm. 40). Kemnaker membutuhkan pertukaran data dua arah berbasis NIK dan identitas usaha untuk program ketenagakerjaan. Pertukaran data dengan Direktorat Jenderal Pajak dan integrasi layanan dengan BPJS Kesehatan juga berkembang, meskipun belum tergambar dalam dokumen arsitektur kondisi saat ini (D2 hlm. 40) [perlu konfirmasi]. Di tingkat nasional, Peraturan Presiden 39/2019 tentang Satu Data Indonesia dan Instruksi Presiden 4/2025 tentang Data Tunggal Sosial dan Ekonomi Nasional menuntut data yang terstandar dan dapat dibagipakaikan lintas instansi (D1 hlm. 1–3). Tuntutan berbagi data ini berjalan bersamaan dengan kewajiban melindunginya. Undang-Undang 27/2022 tentang Pelindungan Data Pribadi mewajibkan pengendali data menetapkan dasar pemrosesan, menjaga keamanan, dan menunjuk pejabat pelindungan data. Peraturan Pemerintah 33/2026 sebagai peraturan pelaksananya akan berlaku 16 Januari 2027 [perlu konfirmasi] dan mengatur antara lain retensi serta minimisasi data. Dengan demikian, BPJS Ketenagakerjaan harus mampu berbagi lebih banyak data sekaligus mengendalikan data tersebut dengan lebih ketat. Gambar 1.2 memperlihatkan mitra utama dalam ekosistem tersebut.

![[G1-2-Ekosistem-Pertukaran-Data.png]]
*Gambar 1.2 Ekosistem Pertukaran Data Ketenagakerjaan dengan ESDP sebagai Poros*


BPJS Ketenagakerjaan sebenarnya telah menyiapkan landasan kebijakan untuk tuntutan ini. Peraturan Direksi Nomor 4/PERDIR.06/042025 tentang Pedoman Tata Kelola Data dan Teknologi Informasi, yang ditetapkan pada 23 April 2025, menyatukan empat peraturan sebelumnya ke dalam satu pedoman (D1 hlm. 1, 62). Pedoman ini menetapkan struktur tata kelola dari Komite Pengarah TI hingga kantor cabang sebagai produsen data (D1 hlm. 7–10). Pedoman ini juga mengatur arsitektur dan pemodelan data, metadata, kualitas data, data warehouse dan big data, master data dan data referensi, keamanan informasi, serta kolaborasi dan pemanfaatan data dengan mitra (D1 hlm. 14–17, 53–58, 64–92). Untuk kerja sama data, pedoman ini mewajibkan setiap perjanjian kerja sama memuat tujuan, metode integrasi, elemen data, dan aspek keamanan (D1 hlm. 53–58). Perjanjian yang sudah berjalan wajib diadendum dalam enam bulan sejak penetapan (D1 hlm. 61–62). Rangkaian tonggak regulasi dan program yang membingkai studi ini dirangkum pada Gambar 1.3. Kebijakannya sudah tersedia. Tantangan berikutnya adalah menerjemahkan kebijakan tersebut ke dalam praktik, peran, dan arsitektur yang berjalan sehari-hari.

![[G1-3-Linimasa-Regulasi.png]]
*Gambar 1.3 Linimasa Regulasi dan Tonggak Pengelolaan Data BPJS Ketenagakerjaan*


Asesmen arsitektur kondisi saat ini menunjukkan bahwa penerjemahan tersebut belum tuntas. Data mengalir dari aplikasi inti dan aplikasi pendukung melalui berbagai pola, yaitu API, middleware, replikasi, SFTP, dan koneksi langsung ke basis data. Aplikasi inti SMILE berperan sebagai simpul integrasi yang berpotensi menjadi titik kegagalan tunggal (D2 hlm. 20–21, 39). Pengelolaan master data masih manual. Big data belum mengacu pada master data, sehingga belum dapat dipastikan menjadi sumber kebenaran tunggal (D2 hlm. 23–24). Definisi istilah dan elemen data berbeda antarbidang, dan kewenangan atas perubahannya belum jelas (D2 hlm. 26). Kolaborasi data dengan mitra berlangsung melalui platform yang beragam sesuai mitra (D2 hlm. 22). Di sisi lain, BPJS Ketenagakerjaan telah memulai sejumlah langkah yang menjadi modal penting. Platform data perusahaan (Enterprise Strategic Data Platform/ESDP) berbasis Cloudera sudah beroperasi. Uji konsep Master Data Management untuk menghasilkan *golden record* peserta sedang berjalan dengan target Oktober 2026 [perlu konfirmasi status]. Data Hub juga direncanakan sebagai jalur tunggal pertukaran data antarlembaga yang terkelola melalui API Management. Langkah-langkah ini baru akan memberi hasil optimal bila disusun dalam satu arsitektur yang utuh. Dalam arsitektur itu ESDP berperan sebagai poros: data dari sistem sumber mengalir masuk ke ESDP, diolah dan dikendalikan di dalamnya, lalu disalurkan ke pengguna internal maupun mitra eksternal dengan standar yang sama.

Atas dasar itu, studi Reviu dan Perancangan Arsitektur Interoperabilitas Data Ketenagakerjaan disusun. Studi ini memotret kondisi pengelolaan data pada wilayah yang dicakup ESDP, mengukur kesenjangannya terhadap kebijakan internal dan tuntutan regulasi, lalu merumuskan arsitektur target dan peta jalan tiga tahun. Peta jalan tersebut diurutkan berdasarkan ketergantungan antarlangkah dan kunci waktu eksternal. Blueprint ini dimaksudkan menjadi acuan bersama manajemen, unit pengelola data, dan unit teknologi informasi agar setiap investasi data, mulai dari Data Hub hingga *golden record* dan lapisan analitik, saling menguatkan dan tidak berjalan sendiri-sendiri. Maksud, tujuan, dan batas ruang lingkup studi diuraikan pada subbab 1.2.

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

**Klaim yang perlu diverifikasi klien**
1. Pertukaran data dengan DJP dan integrasi layanan dengan BPJS Kesehatan (e-PLKK) masih aktif, meskipun tidak tampak pada diagram integrasi eksternal (D2 hlm. 40). Saat ini klaim ini hanya bersumber dari informasi publik.
2. Kemnaker disebut sebagai mitra pertukaran data dua arah. Sumbernya dokumen teknis API Kemnaker yang berklasifikasi terbatas, sehingga yang dikutip hanya pola umumnya. Perlu dipastikan klien bersedia hal ini disebut di dokumen yang dipresentasikan.
3. Penyebutan "ESDP berbasis Cloudera sudah beroperasi" dan "Data Hub direncanakan sebagai jalur tunggal" sesuai pemahaman BPJS TK (rujukan D3/D4 dan sesi wawancara).
4. SMILE sebagai titik kegagalan tunggal adalah penilaian D2 hlm. 39. Perlu dipastikan klien menerima frasa ini muncul di dokumen yang dipresentasikan.

**Data/angka yang perlu dikonfirmasi**
1. Angka kepesertaan dan dana investasi diambil dari Laporan Terintegrasi 2025 (hlm. 14 dan 53; nomor halaman tercetak). Perlu dipastikan klien memakai angka tahun buku 2025 dan bukan data internal yang lebih baru.
2. Tanggal berlaku PP 33/2026 (16 Januari 2027) diturunkan dari Pasal 225, yaitu enam bulan sejak diundangkan. Tanggal pengundangan perlu dicek ulang.
3. Status dan target uji konsep MDM: target Oktober 2026 sudah lewat, dan hasil maupun keputusan lanjutannya belum diketahui.
4. Status adendum PKS sesuai ketentuan peralihan Perdir 4/2025 (batas sekitar Oktober 2025).
5. Rincian segmen peserta aktif pada Gambar 1.1 (PU 27.406.731 + BPU 14.195.182 + Jakon 7.047.512 = 48.649.425, hlm. 53) tidak memisahkan PMI, padahal definisi peserta aktif mencakup PMI. Perlu dikonfirmasi di mana PMI dihitung.
6. Tonggak Data Hub "2028 (Tahun 2 K2)" dan "Tahun 1 = 2027" pada Gambar 1.3 bergantung pada asumsi peta jalan [perlu konfirmasi].
7. Nama resmi "Enterprise Strategic Data Platform" untuk ESDP.

**Keputusan yang perlu diambil ikpu**
1. Mitra mana yang disebut secara eksplisit di paragraf kedua. Pilihannya: semua mitra (Dukcapil, Kemnaker, DJP, BPJS Kesehatan) atau hanya mitra yang terdokumentasi di D2.
2. Apakah angka 2024 (Laporan Terintegrasi 2024) perlu ditampilkan sebagai pembanding tren. Contohnya peserta aktif 45.224.676 pada Desember 2024.
3. Format sitasi untuk dokumen publik (Laporan Terintegrasi) dan dokumen internal (D1/D2): cukup catatan kaki, atau dicantumkan dalam daftar pustaka di 1.4.
4. Gambar 1.2 menandai Kemnaker sebagai mitra terdokumentasi (garis utuh) berdasarkan SiapKerja di D2 hlm. 40, sedangkan DJP, BPJS Kesehatan, dan Satu Data/DTSEN ditandai garis putus-putus. Perlu diputuskan apakah ketiga mitra bergaris putus-putus tetap ditampilkan di versi yang dipresentasikan.
5. Apakah penyebutan regulasi (UU PDP, PP 33/2026, Perpres 39/2019, Inpres 4/2025) di sini cukup ringkas, dengan rincian dipindahkan ke 1.3.
