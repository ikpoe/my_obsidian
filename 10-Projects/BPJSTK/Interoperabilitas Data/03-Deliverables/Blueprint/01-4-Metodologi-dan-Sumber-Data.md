# 1.4 Metodologi dan Sumber Data

## 1.4.1 Tahapan Penyusunan

Blueprint disusun dalam enam tahap. Hasil setiap tahap divalidasi bersama tim BPJS Ketenagakerjaan sebelum tahap berikutnya dimulai (Gambar 1.5).

![[G1-6-Tahapan-Metodologi.png]]
*Gambar 1.5 Tahapan Penyusunan Blueprint*

1. **Kajian dokumen.** Kebijakan internal dibaca sebagai kondisi seharusnya, sedangkan dokumen arsitektur dibaca sebagai kondisi saat ini.
2. **Diskusi dengan tim BPJS Ketenagakerjaan.** Hasil kajian dikonfirmasi untuk menangkap praktik yang tidak tertulis.
3. **Reviu per area pengelolaan data.** Setiap area direviu dengan DAMA-DMBOK sebagai panduan cakupan. Setiap pengamatan harus didukung bukti, dan kebijakan yang belum dijalankan tidak dianggap sudah berjalan.
4. **Analisis kesenjangan.** Kondisi saat ini dibandingkan dengan kebijakan internal, regulasi, dan praktik yang lazim. Kesenjangan yang muncul di beberapa area dirumuskan sebagai tantangan utama.
5. **Perumusan prinsip dan arsitektur target.** Prinsip disusun dari tantangan tersebut dan menjadi dasar rancangan target. Untuk keputusan yang memiliki lebih dari satu pilihan, opsi-opsinya dibandingkan sebelum satu opsi direkomendasikan.
6. **Penyusunan peta jalan.** Inisiatif diurutkan per kuartal selama tiga tahun berdasarkan ketergantungan antarlangkah dan kunci waktu regulasi.

## 1.4.2 Sumber Data

- **Regulasi dan kebijakan internal**, seperti yang tercantum pada dasar hukum.
- **Dokumen BPJS Ketenagakerjaan**, yaitu paparan arsitektur enterprise, dokumen Data Hub, dan materi paparan platform.
- **Dokumen pengadaan**, yaitu kerangka acuan kerja (TOR) dan proposal penawaran.
- **Diskusi** dengan tim BPJS Ketenagakerjaan.
- **Publikasi resmi**, yaitu Laporan Terintegrasi BPJS Ketenagakerjaan.

Rujukan ditulis dengan kode sumber dan nomor halaman, misalnya "(D1 hlm. 53)". Fakta yang belum dapat dipastikan atau hanya bersumber dari satu pihak ditandai [perlu konfirmasi].

---

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Reviu ikpu (2026-11-06):** subbab 1.4.3 Keterbatasan dihapus.

> **Reviu ikpu (2026-11-06, 1.4 ringkas):** 1.4 dipadatkan. Paragraf pembuka dihapus; enam tahap menjadi daftar bernomor satu sampai dua kalimat; sumber data dikelompokkan tanpa rincian kode dan tanggal; aturan sitasi dan penanda dipersingkat; keterbatasan menjadi tiga butir. Rincian D1–D6, R1–R2, dan aturan penanganan dokumen terbatas tetap tersedia di riwayat git dan dapat dipindah ke lampiran bila diperlukan.

> **Penyelarasan TOR (2026-11-05):** TOR dan proposal ditambahkan pada kelompok dokumen pengadaan karena 1.1 dan 1.2 kini mengutip TOR.

> **Pembaruan R2 (2026-11-05):** R2 (diskusi dengan BPJS Ketenagakerjaan, 4 November 2026) ditambahkan ke daftar sumber karena dikutip di subbab lain.

> **Integrasi D6 (2026-11-02):** D6 ditambahkan pada tahap kajian dokumen (sebagai kondisi seharusnya untuk area kualitas data) dan pada kelompok kebijakan internal di 1.4.2.

> **Catatan aset:** `assets/G1-6-Tahapan-Metodologi.svg` (sumber yang dapat disunting; teks asli, grup `<g>` ber-id) dan `.png` (ekspor 220 dpi).

**Keputusan yang perlu diambil ikpu**
1. **Penomoran gambar tidak berurutan.** Subbab 1.1 kini memakai Gambar 1.1–1.2 (Gambar 1.3 linimasa tidak lagi disematkan), sedangkan 1.2 memakai Gambar 1.4, dan subbab ini Gambar 1.5 (dinomori ulang 2026-11-05 setelah gambar tujuan dipindah ke 3.1). Perlu diputuskan apakah semua gambar dinomori ulang setelah BAB I stabil, dan apakah file G1-3 dipakai di 1.3 atau dihapus.
2. Validasi digambarkan berulang pada setiap tahap. Pilihannya mempertahankan ini atau menjadikannya tahap terakhir saja jika reviu klien hanya dilakukan sekali.
3. Penyebutan contoh opsi "pola pemisahan lingkungan platform" pada tahap perumusan arsitektur target. Perlu dipastikan contoh ini sesuai dengan perbandingan opsi yang benar-benar disajikan.
4. **Selesai (2026-11-02):** skala kematangan tidak lagi disebut karena skor tidak dipublikasikan. Lampiran A diganti judul menjadi "Hasil Reviu per Area" (diputuskan ikpu).
5. Dokumen teknis mitra yang bersifat terbatas hanya disebut secara umum dan tidak diberi kode, karena tidak tercantum dalam daftar dokumen studi di 1.3.

**Konfirmasi ke klien atau verifikasi**
1. Tanggal diskusi R1 (27 Oktober 2026) dan tanggal paparan D3 (31 Oktober 2026).
2. Daftar narasumber atau unit yang terlibat dalam diskusi, bila perlu dicantumkan.
3. Apakah klien menghendaki reviu formal (misalnya sesi validasi hasil reviu) sebelum Blueprint final.
