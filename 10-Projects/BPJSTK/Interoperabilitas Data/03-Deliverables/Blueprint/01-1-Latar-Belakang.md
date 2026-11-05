# 1.1 Latar Belakang

BPJS Ketenagakerjaan menempatkan teknologi digital sebagai arah strategis untuk meningkatkan kualitas layanan, efektivitas operasional, dan pengambilan keputusan berbasis data. Di atas arah tersebut, penguatan tata kelola data menjadi fondasi, karena mutu layanan bergantung pada data yang benar dan dapat dipercaya (TOR hlm. 3).

Data strategis BPJS Ketenagakerjaan berasal dari kepesertaan, iuran, dan manfaat, serta dari kerja sama pertukaran data dengan instansi lain (TOR hlm. 3). Skalanya besar. Pada akhir 2025, hampir tujuh puluh juta tenaga kerja tercatat sebagai peserta, dengan dana investasi yang mendekati seribu triliun rupiah (Laporan Terintegrasi 2025, hlm. 14). Pada skala ini, ketidaktepatan data sekecil apa pun berdampak langsung pada hak jutaan pekerja.

![[G1-1-Kepesertaan.png]]
*Gambar 1.1 Kepesertaan dan Dana Investasi BPJS Ketenagakerjaan per 31 Desember 2025*

Kerja sama pertukaran data tersebut terus bertambah, baik dengan kementerian dan lembaga maupun dengan mitra eksternal lainnya. Seiring itu, kompleksitas integrasi data juga meningkat, sehingga interoperabilitas data yang sudah berjalan perlu dievaluasi (TOR hlm. 3). Gambar 1.2 memperlihatkan kelompok mitra dalam ekosistem pertukaran data tersebut.

![[G1-2-Ekosistem-Pertukaran-Data-v6.png]]
*Gambar 1.2 Ekosistem Pertukaran Data Ketenagakerjaan*

Kebutuhan ini sejalan dengan tugas dan fungsi Deputi Bidang Manajemen Data dan Analitik untuk memastikan kualitas data sesuai standar tata kelola data. Kebutuhan ini juga sejalan dengan dua fokus Rencana Strategis BPJS Ketenagakerjaan 2027–2031, yaitu F3 Pusat Interoperabilitas Ketenagakerjaan Nasional dan F1 Penguatan Tata Kelola dan Ekosistem Data (TOR hlm. 3).

Atas dasar itu, diperlukan reviu atas arsitektur interoperabilitas data yang sedang berjalan sebagai dasar perancangan arsitektur interoperabilitas data ketenagakerjaan. Studi ini juga membantu manajemen melihat arsitektur data enterprise secara utuh dan menyeluruh (*end to end*) (TOR hlm. 3).

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Reviu ikpu (2026-11-06, versi TOR):** 1.1 disusun ulang mengikuti alur latar belakang TOR hlm. 3 (arah digital dan tata kelola data, sumber data strategis dan kompleksitas integrasi, dasar tugas Deputi dan Renstra F3/F1, kebutuhan reviu, pandangan *end to end*), lalu dipoles dan ditambah data kepesertaan (Gambar 1.1) serta ekosistem mitra (Gambar 1.2). Dokumen Renstra 2027–2031 belum tersedia dan masih disusun per Juni 2026; rujukan F1/F3 hanya dari TOR.

> **Reviu ikpu (2026-11-06, ringkas):** 1.1 dipadatkan menjadi pernyataan masalah: kebutuhan interoperabilitas, skala data, tuntutan yang bertambah, masalah saat ini, lalu alasan studi. Butir manfaat, rincian segmen kepesertaan, target 2026, dan uraian isi studi dihapus dari 1.1 (isi studi sudah ada di 1.2).

> **Reviu ikpu (2026-11-06):** paragraf pembuka ditulis ulang agar alurnya menyatu. Kata "sebenarnya" dan kalimat "Asesmen arsitektur kondisi saat ini" dihapus. Rincian Prosedur Pengelolaan Kualitas Data (D6) dan kondisi teknis (SMILE, master data, definisi istilah) dikeluarkan dari latar belakang karena sudah dibahas di BAB II. Paragraf kebijakan kini menjelaskan alasan reviu, tanpa mengklaim gambaran kebijakan yang lengkap.

> **Reviu internal (2026-11-06):** (A, B) ditambah butir manfaat pertukaran data: manfaat tepat sasaran termasuk sinkronisasi peristiwa hidup seperti data kematian untuk klaim [perlu konfirmasi], ketepatan sasaran program pemerintah lintas instansi (Dukcapil, Kemnaker, BPJS Kesehatan), kebijakan berbasis bukti, dan kepatuhan UU PDP. Tujuan dokumen tidak diubah. (J) DTSEN dihapus dari teks dan Gambar 1.2 karena tidak ada pertukaran dengan DTSEN; Satu Data disebut hanya menerima data massal [perlu konfirmasi]. Gambar 1.2 kini berkas v2 (`G1-2-Ekosistem-Pertukaran-Data-v6.svg/.png`), kotak Satu Data tanpa DTSEN dan label tengah "ESDP sebagai pusat interoperabilitas". Kalimat ESDP di paragraf akhir diselaraskan dengan R2 (jalur dapat berbeda, tata kelola sama). Klaim "18 elemen", pusat data mandiri, dan BSU tidak dimasukkan karena belum bersumber.

> **Penyelarasan TOR (2026-11-05):** nama ESDP dikoreksi menjadi Enterprise Service Data Platform (TOR hlm. 4). Kalimat "ESDP berbasis Cloudera sudah beroperasi" diganti menjadi "platform data berbasis Cloudera sudah beroperasi", karena TOR memperlakukan ESDP dan Cloudera sebagai komponen terpisah (TOR hlm. 4–5). Dasar Renstra F3 dan F1 ditambahkan.

> **Pembaruan R2 (2026-11-05):** Data Hub tidak lagi disebut sebagai jalur tunggal. Pertukaran *real-time* boleh langsung dari sistem inti di bawah tata kelola yang sama, berdasarkan diskusi dengan BPJS Ketenagakerjaan pada 4 November 2026 (R2).

> **Integrasi D6 (2026-11-02):** satu kalimat tentang Prosedur Pengelolaan Kualitas Data (D6 hlm. 4) ditambahkan pada paragraf landasan kebijakan. Fakta lain tidak berubah.

> **Catatan aset:** sumber gambar yang dapat disunting (SVG, teks asli, font Arial/Helvetica) tersedia di `assets/` (G1-1, G1-2 `.svg`). PNG pada dokumen ini diekspor dari SVG tersebut pada 220 dpi. Perubahan gambar dilakukan pada SVG, lalu PNG diekspor ulang.

**Klaim yang perlu diverifikasi klien**
1. Pertukaran data dengan DJP dan integrasi layanan dengan BPJS Kesehatan (e-PLKK) masih aktif, meskipun tidak tampak pada diagram integrasi eksternal (D2 hlm. 40). Saat ini klaim ini hanya bersumber dari informasi publik.
2. Kemnaker disebut sebagai mitra pertukaran data dua arah. Sumbernya dokumen teknis API Kemnaker yang berklasifikasi terbatas, sehingga yang dikutip hanya pola umumnya. Perlu dipastikan klien bersedia hal ini disebut di dokumen yang dipresentasikan.
3. ~~ESDP berbasis Cloudera~~ dikoreksi sesuai TOR (lihat penyelarasan TOR). Penyebutan dan "Data Hub direncanakan sebagai jalur bersama" sesuai pemahaman BPJS TK (rujukan D3/D4 dan sesi wawancara).
4. SMILE sebagai titik kegagalan tunggal adalah catatan D2 hlm. 39. Perlu dipastikan klien menerima frasa ini muncul di dokumen yang dipresentasikan.

**Data/angka yang perlu dikonfirmasi**
1. Angka kepesertaan dan dana investasi diambil dari Laporan Terintegrasi 2025 (hlm. 14 dan 53; nomor halaman tercetak). Perlu dipastikan klien memakai angka tahun buku 2025 dan bukan data internal yang lebih baru.
2. ~~Tanggal berlaku PP 33/2026~~ Dipindahkan ke Catatan Review 1.3.
3. Status dan target uji konsep MDM: target Oktober 2026 sudah lewat, dan hasil maupun keputusan lanjutannya belum diketahui.
4. ~~Status adendum PKS~~ Dipindahkan ke Catatan Review 1.3.
5. Rincian segmen peserta aktif pada Gambar 1.1 (PU 27.406.731 + BPU 14.195.182 + Jakon 7.047.512 = 48.649.425, hlm. 53) tidak memisahkan PMI, padahal definisi peserta aktif mencakup PMI. Perlu dikonfirmasi di mana PMI dihitung.
6. [Selesai 2026-11-02: Gambar 1.3 linimasa regulasi dikeluarkan dari 1.1; materinya milik 1.3.] Tonggak Data Hub "2028 (Tahun 2 K2)" dan "Tahun 1 = 2027" pada gambar linimasa bergantung pada asumsi peta jalan [perlu konfirmasi].
7. **Selesai (2026-11-05):** nama resmi ESDP adalah "Enterprise Service Data Platform" (TOR hlm. 4).

**Keputusan yang perlu diambil ikpu**
1. Paragraf pembuka kini menempatkan interoperabilitas data sebagai fokus utama, sesuai judul kegiatan. Apakah urutan ini (pertukaran data lebih dulu, baru skala kepesertaan) sudah tepat?
2. Teks di dalam Gambar 1.2 dan Gambar 1.4 mungkin masih memakai istilah lama pada judulnya. Keterangan gambar sudah diganti menjadi "ESDP sebagai Pusat"; SVG perlu dicek dan diekspor ulang bila perlu.
3. **Selesai (2026-11-05):** `00-Admin/01-TOR.md` dilengkapi dari TOR asli. Pembingkaian interoperabilitas sejalan dengan latar belakang TOR (hlm. 3). Batas peran ESDP dan Cloudera masih perlu dikonfirmasi.
4. Mitra mana yang disebut secara eksplisit di paragraf ketiga. Pilihannya: semua mitra (Dukcapil, Kemnaker, DJP, BPJS Kesehatan) atau hanya mitra yang terdokumentasi di D2.
5. Apakah angka 2024 (Laporan Terintegrasi 2024) perlu ditampilkan sebagai pembanding tren. Contohnya peserta aktif 45.224.676 pada Desember 2024.
6. Format sitasi untuk dokumen publik (Laporan Terintegrasi) dan dokumen internal (D1/D2): cukup catatan kaki, atau dicantumkan dalam daftar pustaka di 1.4.
7. Gambar 1.2 menandai Kemnaker sebagai mitra terdokumentasi (garis utuh) berdasarkan SiapKerja di D2 hlm. 40, sedangkan DJP, BPJS Kesehatan, dan Satu Data Indonesia ditandai garis putus-putus. Perlu diputuskan apakah ketiga mitra bergaris putus-putus tetap ditampilkan di versi yang dipresentasikan.
8. **Selesai (2026-11-02):** rincian regulasi dipindahkan ke 1.3. Subbab 1.1 hanya menyebut alasan regulasi relevan dan merujuk ke 1.3.
