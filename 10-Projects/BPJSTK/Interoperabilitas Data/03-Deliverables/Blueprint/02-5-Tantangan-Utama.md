# 2.5 Tantangan Utama

Subbab ini merumuskan tantangan yang membuat pengembangan pengelolaan data dan ESDP tidak mudah dijalankan. Tantangan dikelompokkan menurut orang, proses, teknologi, dan data.

![[G2-5-Tantangan-Kelompok.png]]
*Gambar 2.5 Tantangan Utama per Kelompok*

## Orang dan Organisasi

### Tantangan 1. Menggeser pekerjaan data dari satu tim ke banyak unit

- Saat ini kebutuhan analitik dilayani lewat permintaan ke tim data (D2 hlm. 22). Peran baru seperti owner dan steward di unit bisnis akan menambah tanggung jawab di luar tugas pokok mereka.
- Perbaikan data di hulu bergantung pada petugas entri dan kantor cabang, yang kesadarannya masih rendah (D2 hlm. 28).
- Unit pengelola data relatif baru (D2 hlm. 51), sehingga kapasitas untuk mengawal perubahan sekaligus melayani kebutuhan harian masih terbatas [perlu konfirmasi].

## Proses dan Tata Kelola

### Tantangan 2. Menyepakati sumber data yang dipercaya

- Menentukan sumber yang dipercaya untuk tiap atribut menuntut kesepakatan lintas direktorat, bukan hanya pekerjaan teknis (D2 hlm. 24; R1).
- Selama kesepakatan belum ada, keputusan atas kasus data tertentu cenderung diambil satu per satu (R1).

### Tantangan 3. Menjadikan pertukaran data dengan mitra satu pintu

- Kebutuhan pertukaran dengan kementerian, lembaga, dan mitra lain terus berjalan (R1), tetapi pengajuannya belum satu pintu (R1) dan jalur teknisnya berbeda-beda (D2 hlm. 22, 40).
- Ketentuan PKS sudah rinci (D1 hlm. 53–58), sedangkan Data Hub sebagai jalur bersama belum dibangun (D4). Jalur perlu diseragamkan tanpa menghentikan pertukaran yang sudah berjalan.

## Teknologi

### Tantangan 4. Berubah sambil layanan tetap berjalan

- SMILE melayani seluruh direktorat, aplikasinya monolitik, dan menjadi hub integrasi bagi banyak aplikasi lain (D2 hlm. 35, 37, 39). Setiap perubahan pada jalur data berisiko langsung terasa pada layanan peserta.
- Pengembangan perlu dilakukan bertahap dan berdampingan dengan sistem lama, sehingga untuk sementara ada dua cara kerja yang harus dijaga konsisten.

### Tantangan 5. Memodernisasi platform tanpa menambah fragmentasi

- Banyak keputusan teknologi saling bergantung: pemisahan environment data lake dan DWH (D2 hlm. 21), kesiapan DRC [perlu konfirmasi], dua katalog metadata (D2 hlm. 26), dan dua alat BI (D2 hlm. 22).
- Menambah alat baru tanpa rencana terpadu berisiko menambah silo. Keputusan perlu diurutkan agar investasi saling menguatkan.

## Data dan Regulasi

### Tantangan 6. Menyatukan data dari banyak kanal

- Data peserta dan pemberi kerja datang dari banyak kanal, misalnya SMILE, JMO, POM, dan Lapak Asik, dengan kualitas dan kode referensi yang berbeda (D2 hlm. 26, 28; D3).
- Duplikasi dan format tidak valid harus dibereskan pada data yang terus bertambah setiap hari, bukan sekali jalan (D2 hlm. 28).

### Tantangan 7. Melindungi data pribadi yang disimpan sangat lama

- Data kepesertaan dapat disimpan lebih dari 40 tahun (R1), sementara kepatuhan UU PDP menjadi prioritas (R1).
- Retensi, minimisasi, dan pengendalian akses harus diterapkan pada volume data historis yang besar, termasuk data di platform analitik yang dibangun sebelum aturan ini ada.
- Peraturan direksi tentang pelindungan data pribadi yang dirujuk D1 belum tersedia [perlu konfirmasi], sehingga aturan teknisnya belum punya pegangan.

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Catatan aset:** `assets/G2-5-Tantangan-Kelompok.excalidraw.md` (sumber yang dapat disunting di Obsidian Excalidraw, compressed-json, id teks 8 karakter, kisi 2×2 per kelompok) dan `assets/G2-5-Tantangan-Kelompok.png` (ekspor lokal skala 2×, tidak lewat layanan daring). Nomor gambar mengikuti nomor subbab, sehingga tidak ada Gambar 2.4.

**Keputusan yang perlu diambil ikpu**
1. Keselarasan dengan 2.4: tantangan 1, 2, dan 5 bersinggungan dengan tema lintas area di 2.4 (kepemilikan data, titik tunggal, jalur keluar data). Apakah pembedaannya (2.4 kondisi, 2.5 hal yang membuat perubahan sulit) sudah cukup, atau tema di 2.4 perlu dipangkas?

**Konfirmasi ke klien atau verifikasi**
1. Rencana modernisasi atau pemecahan SMILE dalam beberapa tahun ke depan, karena memengaruhi tantangan 4.
2. Kapasitas tim Deputi Manajemen Data dan Analitik saat ini dan rencana penambahannya.
