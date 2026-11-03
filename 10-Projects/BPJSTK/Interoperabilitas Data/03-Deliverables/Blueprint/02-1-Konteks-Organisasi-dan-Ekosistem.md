# 2.1 Konteks Organisasi dan Ekosistem Data Ketenagakerjaan

BPJS Ketenagakerjaan menyelenggarakan lima program jaminan sosial: JKK, JKM, JHT, JP, dan JKP. Hampir semua data peserta, pemberi kerja, iuran, dan klaim lahir dari layanan program tersebut. Skalanya besar dan tersebar. Tahun 2025, operasinya dijalankan 11 kantor wilayah dan 325 kantor cabang dengan 5.194 karyawan (Laporan Terintegrasi 2025 hlm. 6, 173). Di sisi layanan, manfaat yang dibayarkan mencapai Rp68.247,05 miliar untuk 5.772.567 kasus klaim, dan sekitar dua pertiganya kasus JHT (hlm. 65).

![[G2-1-Fakta-Kunci-Organisasi.png]]
*Gambar 2.1 Fakta Kunci Organisasi dan Layanan Klaim 2025*

## Struktur pengelolaan data

Perdir 4/2025 membagi peran tata kelola data dan TI ke beberapa lapis (D1 hlm. 7):
- **Direktur yang membidangi data dan TI**, sebagai penanggung jawab tertinggi.
- **KPTI dan UPTI**, sebagai komite dan unit pelaksana teknologi informasi.
- **Unit Bisnis Kantor Pusat**, sebagai pemilik proses dan data program.
- **Kantor Wilayah dan Kantor Cabang**, yang tidak hanya memakai data, tetapi juga ikut menghasilkan data. Kancab ditetapkan sebagai produsen data (D1 hlm. 10).

Di tingkat organisasi, fungsi data berada di bawah Deputi Bidang Manajemen Data dan Analitik dengan empat asisten deputi: Data Governance, Interoperability & Processing, Data Analytics, dan Data Collaboration (D2 hlm. 51). Prosedur kualitas data juga sudah menetapkan peran Business Owner, Data Quality, dan Data Liaison (D6). Artinya, perangkat organisasinya sudah ada. Pekerjaan yang tersisa adalah menjalankan peran-peran ini secara konsisten di seluruh kantor.

## Ekosistem data

Data BPJS Ketenagakerjaan bergerak melalui tiga lingkaran:
- **Kanal layanan internal.** Selain aplikasi inti, ada JMO, SIPP Online, Lapak Asik, E-Klaim PMI, PERISAI, E-Jakon, Kiosk, dan Contact Center. Setiap kanal menangkap data peserta atau pemberi kerja dari sisinya sendiri (D2).
- **Mitra pemerintah (K/L).** Dukcapil untuk verifikasi kependudukan, SISKOP2MI untuk pekerja migran, dan SiapKerja milik Kemnaker (D2 hlm. 40). Pertukaran dengan DJP dan BPJS Kesehatan juga disebut di sumber publik [perlu konfirmasi].
- **Mitra non-K/L.** Terdiri atas perbankan, kanal pembayaran, dan penyedia layanan (D2 hlm. 40). Nama yang tercantum antara lain:
  - Pajakku
  - Tanamduit
  - Jasnita
  - BSrE
  - Kanal Mitra

Perdir 4/2025 membedakan mitra K/L dan non-K/L. Kolaborasi data dilakukan untuk mendukung Satu Data Indonesia, perluasan program, peningkatan layanan, dan kualitas data (D1 hlm. 53). Skemanya mencakup verifikasi data dan data sebagai layanan (D1 hlm. 54). Pertukaran melalui Data Hub mensyaratkan PKS dan dokumen teknis sebelum mitra diberi akses (D4).

Dengan banyaknya kanal dan mitra, satu entitas yang sama (individu ber-NIK, peserta ber-KPJ, perusahaan ber-NIB/NPP, dan kantor) dicatat di banyak tempat. Karena itu ESDP diposisikan sebagai titik pusat integrasi data: data dikelola sekali, lalu dipakai bersama oleh unit internal dan mitra sesuai hak aksesnya.

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Catatan aset:** `assets/G2-1-Fakta-Kunci-Organisasi.svg` (sumber yang dapat disunting; teks asli, grup `<g>` ber-id) dan `.png` (ekspor 220 dpi).

**Keputusan yang sudah diambil (2026-11-03)**
- Jumlah kantor cabang: ikpu memilih 325 (Laporan Terintegrasi 2025 hlm. 173) pada 2026-11-03. Catatan hitungan: di laporan yang sama angka 318 muncul satu kali (hlm. 203, profil organisasi) dan 325 juga satu kali (hlm. 173, wilayah wajib lapor LHKPN).
- Angka kepesertaan (terdaftar, aktif, pemberi kerja) cukup tampil di Gambar 1.1. Kalimat kepesertaan dan kolom kepesertaan pada Gambar 2.1 dihapus.
- Mitra non-K/L disebut kategorinya dulu, lalu daftar namanya.

**Keputusan yang perlu diambil ikpu**
1. ~~Kanal Mitra pada D2 hlm. 40 bisa berarti kategori, bukan nama mitra. Tetap dimasukkan ke daftar atau dihapus?~~ Diputuskan (2026-11-03): tetap masuk dulu.

**Konfirmasi ke klien atau verifikasi**
1. Apakah pertukaran data dengan DJP dan BPJS Kesehatan masih aktif, karena tidak tampak di diagram integrasi D2 hlm. 40?
