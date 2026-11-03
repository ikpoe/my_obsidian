# 2.2 Gambaran Lanskap Data dan Teknologi Saat Ini

Subbab ini memotret kondisi data dan teknologi BPJS Ketenagakerjaan apa adanya, dari aplikasi sumber sampai pengguna data.

![[G2-2-Lanskap-Data-dan-Teknologi.png]]
*Gambar 2.2 Lanskap Data dan Teknologi Saat Ini*

## Aplikasi dan basis data sumber

- **SMILE adalah sistem inti** yang dipakai di seluruh direktorat (D2 hlm. 35). Aplikasinya monolitik dan berjalan di WebLogic (D2 hlm. 37).
- **Aplikasi dan kanal lain** jumlahnya banyak, antara lain JMO, SIPP Online, Lapak Asik, E-PLKK, E-Klaim PMI, PERISAI, E-Jakon, Kiosk, Contact Center, dan HCIS. Sebagian terhubung ke SMILE, sebagian berdiri sendiri (D2 hlm. 35, 45). Masih ada delapan proses bisnis yang belum didukung aplikasi dan dikerjakan dengan Google Workspace (D2 hlm. 36).
- **Basis data beragam.** Oracle dipakai 30 aplikasi, diikuti MariaDB (9), PostgreSQL (2), Yugabyte (2), dan SQL Server (1) (D2 hlm. 38).

## Integrasi data

- **Ke platform data**, data ditarik dengan Qlik Replicate (CDC dari log Oracle), Talend (ETL), Oracle ADG sebagai standby database untuk staging, serta API dan SFTP (D2 hlm. 20–21). Frekuensi CDC paling cepat per jam, dan streaming belum dipakai (R1).
- **Antaraplikasi**, pertukaran memakai API, middleware/web service, dan akses langsung ke basis data. SMILE menjadi hub integrasinya (D2 hlm. 39).
- **Dengan pihak luar**, koneksi berupa host-to-host dan API, sebagian besar ke SMILE dan bank. Polanya belum seragam karena tiap aplikasi berintegrasi sendiri-sendiri (D2 hlm. 40). Pertukaran rutin juga berjalan dengan Kemenkes dan BPJS Kesehatan, termasuk data JKP melalui SFTP terenkripsi (R1).
- **Data Hub** disiapkan sebagai jalur pertukaran dengan mitra: REST/JSON di DMZ, diekspos melalui API Management Red Hat 3scale (D4). API Gateway dikelola tim infrastruktur dan sebagian layanan sudah melewatinya (R1).

## Penyimpanan, metadata, dan master data

- **Platform data berbasis Cloudera** menampung data lake dan data warehouse (termasuk EDW dan data mart) dalam satu environment (D2 hlm. 20–21). Edisi dan versi Cloudera yang dipakai [perlu konfirmasi].
- **Data tidak terstruktur** sudah masuk rancangan, tetapi belum berjalan (D2 hlm. 20).
- **Metadata** dikelola di dua katalog terpisah: Dataedo untuk Oracle dan Apache Atlas untuk Cloudera (D2 hlm. 26).
- **Master data** masih dikelola manual, tanpa tool khusus (D2 hlm. 24). Uji konsep MDM untuk entitas kepesertaan sedang berjalan (D3).

## Analitik dan pemanfaatan

- Analitik deskriptif, diagnostik, dan prediktif sudah berjalan berbasis use case dengan Python dan KNIME. Analitik preskriptif belum berjalan (D2 hlm. 20, 22).
- Hasilnya disajikan lewat Tableau dan QlikSense dalam 17 dasbor. Kebutuhan analisis baru diajukan sebagai permintaan ke tim data (D2 hlm. 22).

## Infrastruktur

- Server berada on-premise dengan virtualisasi dan container (D2 hlm. 50).
- Pusat data utama (DC) dan cadangan (DRC) memakai colocation Tier III (D2 hlm. 50).
- Backup disimpan di disk dan tape, dengan tape disimpan di luar lokasi (D2 hlm. 50).

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Catatan aset:** `assets/G2-2-Lanskap-Data.excalidraw.md` (sumber yang dapat disunting di Obsidian Excalidraw, compressed-json, id teks 8 karakter; lajur dan pita dikelompokkan) dan `assets/G2-2-Lanskap-Data-dan-Teknologi.png` (ekspor lokal skala 2×, tidak lewat layanan daring).

> **Batas cakupan:** subbab ini hanya deskriptif. Risiko dan kesenjangan (SMILE sebagai titik kegagalan tunggal, data lake dan DWH dalam satu environment, backup tanpa uji restore berkala, dua katalog metadata) sengaja disimpan untuk 2.3 dan 2.5.

**Keputusan yang perlu diambil ikpu**
1. Apakah batas cakupan di atas sudah tepat, atau 2.2 perlu menyebut risiko utama secara singkat?
2. Apakah produk (Qlik Replicate, Talend, Red Hat 3scale, Tableau, dsb.) boleh disebut namanya di dokumen yang dipresentasikan, atau cukup jenis teknologinya?

**Konfirmasi ke klien atau verifikasi**
1. Edisi dan versi Cloudera yang dipakai.
2. Status API Management: R1 menyebut API Gateway sudah dipakai sebagian layanan, sedangkan D3 menandainya belum tersedia. Mana yang berlaku?
