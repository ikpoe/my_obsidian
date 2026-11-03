# 2.3 Kondisi Saat Ini per Area

Subbab ini merangkum hasil reviu kondisi saat ini untuk sembilan area pengelolaan data. Setiap area memuat apa yang sudah ada dan celah utamanya, terutama terhadap ketentuan Perdir 4/2025.

![[G2-3-Kondisi-per-Area.png]]
*Gambar 2.3 Ringkasan Kondisi Saat Ini per Area*

### Tata Kelola Data

- **Struktur formal sudah ada, tetapi berorientasi TI.** KPTI dibentuk dan bersidang rutin, tetapi tugasnya berpusat pada rencana dan inisiatif TI. Forum keputusan khusus data belum ada (D1 hlm. 7–9).
- **Peran data owner, data steward, dan data custodian belum didefinisikan.** D1 baru menyebut penetapan kepemilikan master data, data liaison, dan kantor cabang sebagai produsen data (D1 hlm. 10, 87, 89). Kewenangan mengubah elemen data juga belum jelas (D2 hlm. 26).
- **Unit pengelola data sudah terbentuk** di bawah Deputi Manajemen Data dan Analitik, termasuk Asdep yang menangani tata kelola data (D2 hlm. 51). Namun topik tata kelola data belum rutin masuk pembahasan (R1).
- **Berbagi data dengan pihak luar belum satu pintu.** Ketentuan PKS sudah rinci (D1 hlm. 53–58), tetapi SOP pertukaran data belum ada dan unit kerja dapat mengajukan PKS sendiri (R1).
- **Kebijakan retensi belum ada**, dan keputusan atas kasus data tertentu, misalnya penggabungan perusahaan, masih diambil kasus per kasus (R1; D3).

### Arsitektur Data

- **Alur lapisan sudah lengkap di rancangan**, dari sumber, integrasi, penyimpanan, dan analitik sampai konsumsi, ditambah lapisan keamanan dan tata kelola (D2 hlm. 20).
- **Data lake dan DWH berada dalam satu environment.** D2 sendiri mencatat risikonya: dampak serangan menjadi lebih luas (D2 hlm. 21).
- **Sebagian komponen referensi D1 belum tampak**, yaitu ODS dan master data sebagai rujukan DWH (D1 hlm. 77). Apakah standby database untuk staging berperan sebagai ODS [perlu konfirmasi].
- **Sisi sumber terpusat pada SMILE.** Aplikasinya monolitik dan menjadi hub integrasi sehingga berpotensi menjadi titik kegagalan tunggal. Akses langsung ke basis data juga masih dipakai (D2 hlm. 37, 39).
- **Dokumentasi arsitektur tertinggal.** Data Building Block masih mengikuti ITMP 2019–2024 (D2 hlm. 31), padahal D1 mewajibkan arsitektur data yang terpelihara (D1 hlm. 14–15).

### Pemodelan Data

- **Model konseptual sudah dimulai**: Data Building Block level 0–3 (D2 hlm. 15–18), EDM per domain yang baru sebagian (D2 hlm. 29–30), dan model konseptual Kepesertaan berisi 13 entitas dari uji konsep MDM (D3). Program JKP belum tercakup (R1).
- **Sebagian pemodelan masih memakai spreadsheet** (D2 hlm. 31).
- **Kewajiban D1 atas model fisik dan reviunya terhadap EDM** (D1 hlm. 16–17) belum terlihat buktinya [perlu konfirmasi].
- **Pola model di platform data tidak terdokumentasi**, termasuk pendekatan DWH, penanganan data historis, dan standar penamaan. Data di platform juga belum mengacu ke master data (D2 hlm. 24).

### Penyimpanan, Retensi, dan Resiliensi

- **Data hasil CDC masuk ke Kudu per jam**, lalu dibentuk menjadi Parquet dengan jeda sekitar 1–2 jam. Data mart sebagian besar berupa tabel hasil job Talend (R1).
- **Status DRC untuk platform data belum jelas.** D3 menggambarkan DRC aktif, sedangkan R1 menyebut platform data di DRC belum ada [perlu konfirmasi].
- **Backup tape off-site sudah ada, tetapi uji restore berkala belum** (D2 hlm. 50).
- **Retensi, arsip, dan tiering belum diatur**, padahal data kepesertaan dapat disimpan lebih dari 40 tahun (R1; D3). D1 sudah memuat ketentuan backup dan pengarsipan (D1 hlm. 90–91).
- **Data tidak terstruktur belum dimanfaatkan secara operasional** (D2 hlm. 20).

### Integrasi dan Interoperabilitas

- **Alat integrasi ke platform data sudah tersedia**: CDC, ETL, standby database, API, dan SFTP (D2 hlm. 20–21). Namun baru sebagian Data Building Block yang tersimpan lengkap (D2 hlm. 21), dan belum ada ukuran kelengkapan ingest, target freshness, data contract, maupun rekonsiliasi.
- **Penyaluran data ke pengguna paling lemah.** Kebutuhan internal dilayani lewat permintaan ke tim data, sedangkan kolaborasi dengan mitra memakai platform eksternal yang beragam (D2 hlm. 22).
- **Kebijakan pertukaran berfokus pada keamanan dan PKS**: web service atau SFTP, NDA, dan pembatasan akses (D1 hlm. 50–51). Standar API, katalog layanan, dan SLA antarmuka belum diatur.
- **Integrasi eksternal belum seragam** karena tiap aplikasi berintegrasi sendiri-sendiri (D2 hlm. 40).
- **Data Hub sudah dirancang** dengan API Management (D4), tetapi belum dibangun, dan API Management masih ditandai belum tersedia (D3).

### Data Induk

- **Kesenjangan kebijakan dan praktik paling lebar di area ini.** D1 mewajibkan satu repositori master data terpusat (D1 hlm. 88), sedangkan praktiknya masih manual tanpa SOP maupun tool khusus (D2 hlm. 24).
- **Sumber kebenaran tunggal belum jelas** dan kode referensi berbeda antarsistem (D2 hlm. 24, 26). Duplikasi data peserta masih ditemukan (D2 hlm. 28).
- **Uji konsep MDM kepesertaan sedang berjalan** dengan pembobotan sumber dari SMILE, JMO, POM, dan Lapak Asik (D3; R1). Aturan match/merge dan pemilihan nilai terbaik per atribut belum ditetapkan [perlu konfirmasi].
- **Aturan validasi dasar sudah ada di D1**, misalnya NIK 16 digit (D1 hlm. 84).

### Metadata dan Kualitas Data

- **Metadata tersebar di dua katalog**: Dataedo untuk Oracle dan Apache Atlas untuk Cloudera (D2 hlm. 26). Dataedo cenderung dipakai pasif (R1), dan lineage menyeluruh belum ada (R1; D3).
- **Definisi data belum seragam.** Nama dan definisi yang sama bisa berbeda antardokumen, dan perubahan struktur di sumber tidak terdeteksi alat (D2 hlm. 26).
- **Prosedur kualitas data sudah ada** (D2 hlm. 27; D6), lengkap dengan siklus pengelolaan di D1 (D1 hlm. 80–85). Namun duplikasi dan format tidak valid masih muncul, perbaikan di kantor cabang belum optimal, dan kesadaran petugas entri masih rendah (D2 hlm. 28).
- **Kontrol kualitas di pipeline belum ada**, baik quality gate per lapisan maupun target freshness. Sebagian data iuran masih diolah dengan Excel (R1).

### Keamanan dan Pelindungan Data

- **Kebijakan keamanan cukup lengkap**: 16 kontrol SMKI (D1 hlm. 39–40), klasifikasi Biasa, Terbatas, dan Rahasia (D1 hlm. 93–94), matriks akses CRUD (D1 hlm. 101), enkripsi AES-256 (D1 hlm. 45), dan ketentuan log (D1 hlm. 107).
- **Bukti penerapan di tingkat data masih minim.** D2 hanya menggambarkan lapisan keamanan data secara umum (D2 hlm. 20). Penandaan klasifikasi per elemen, kontrol akses terpusat di platform, masking, dan audit akses belum terlihat [perlu konfirmasi].
- **Kontrol pertukaran eksternal paling matang**, dengan WAF dan API Security di perimeter (D2 hlm. 37) serta SFTP terenkripsi (R1). Meski begitu, D2 tetap mencatat risiko kebocoran pada transfer data ke luar (D2 hlm. 21–22).
- **Kepatuhan UU PDP menjadi prioritas** (R1), tetapi peraturan direksi tentang pelindungan data pribadi yang dirujuk D1 belum diterima [perlu konfirmasi].

### DW dan BI

- **Data mart dibentuk per kebutuhan bisnis**, dan ada banyak cube untuk aplikasi keuangan (R1). Model lapisan gold belum terdokumentasi.
- **Dua alat BI dipakai tanpa lapisan metrik terpusat.** Tableau dan QlikSense menyajikan 17 dasbor (D2 hlm. 22), tetapi definisi KPI bersama belum ditemukan.
- **Permintaan analitik terpusat di tim data dan butuh waktu lama** (D2 hlm. 22). Self-service belum berjalan, padahal termasuk prinsip di D1 (D1 hlm. 76).
- **Jalur konsumsi bercampur.** Sebagian sistem inti membaca langsung dari platform data (R1).
- **Kebutuhan analitik ke depan sudah jelas**, antara lain forecasting, risiko likuiditas, dan dasbor dengan dimensi waktu yang konsisten (R1).

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Catatan aset:** `assets/G2-3-Kondisi-per-Area.excalidraw.md` (sumber yang dapat disunting di Obsidian Excalidraw, compressed-json, id teks 8 karakter, kartu per area dikelompokkan) dan `assets/G2-3-Kondisi-per-Area.png` (ekspor lokal skala 2×, tidak lewat layanan daring).

**Keputusan yang perlu diambil ikpu**
1. Informasi dari dokumen terbatas sengaja tidak dimuat. Apakah cukup begitu, atau perlu satu kalimat umum tentang kasus akses langsung oleh mitra?
2. Nama produk (Dataedo, Talend, Tableau, dsb.) tetap disebut. Apakah ini sesuai untuk dokumen yang dipresentasikan?

**Konfirmasi ke klien atau verifikasi**
1. Status DRC untuk platform data: D3 menyebut aktif, sedangkan R1 menyebut belum ada.
2. Bukti penerapan keamanan di tingkat data (penandaan klasifikasi, masking, audit akses) dan status peraturan direksi tentang PDP.
