# 1.5 Sistematika Dokumen

Blueprint ini terdiri atas enam bab dan lima lampiran.

- **BAB I Pendahuluan**: latar belakang, maksud, tujuan, ruang lingkup, dasar hukum, dan metodologi.
- **BAB II Kondisi Saat Ini dan Tantangan**: konteks organisasi, lanskap data dan teknologi, kondisi per area, dan tantangan utama.
- **BAB III Arah Pengembangan Data**: visi, kondisi target, prinsip arsitektur, *use case* prioritas, dan peta jalan tiga tahun.
- **BAB IV Arsitektur Interoperabilitas dan Data Target**: rancangan interoperabilitas data sebagai inti, beserta fondasi data yang menopangnya.
- **BAB V Tata Laksana Implementasi**: organisasi pelaksana, risiko dan mitigasi, serta pemantauan.
- **BAB VI Penutup**: rangkuman dan langkah awal.

Lampiran memuat hasil reviu per area, perbandingan opsi, daftar inisiatif, pertanyaan klarifikasi, dan glosarium.

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Reviu ikpu (2026-11-06, 1.5 ringkas):** setiap bab diringkas menjadi satu baris; rincian subbab BAB III/IV dan deskripsi tiap lampiran dihapus.

> **Penggabungan tujuan (2026-11-06):** deskripsi BAB III disesuaikan karena tujuan pengembangan dan kondisi target kini satu daftar K1–K6.

> **Penyelarasan TOR (2026-11-05):** isi subbab tidak diubah; tidak ada pernyataan yang bertentangan dengan TOR. Celah struktur dicatat pada Keputusan 6.

> **Pembaruan R2 (2026-11-05):** deskripsi Data Hub di BAB IV disesuaikan: jalur bersama untuk sebagian besar pertukaran, bukan satu-satunya jalur (R2).

**Keputusan yang perlu diambil ikpu**
1. **Selesai (2026-11-03):** subbab 2.4 diganti judul menjadi "Gambaran Kondisi Secara Umum" dan deskripsi BAB II disesuaikan (diputuskan ikpu).
2. Apakah Ringkasan Eksekutif perlu disebut dalam sistematika?
3. Deskripsi BAB VI ("langkah awal yang perlu segera diambil") masih dugaan karena bab tersebut belum ditulis. Perlu dicek ulang setelah BAB VI selesai.
4. Apakah Lampiran D (pertanyaan klarifikasi terbuka) tetap dilampirkan pada versi yang diserahkan ke klien?
5. BAB IV disusun ulang agar interoperabilitas menjadi blok terbesar (delapan subbab), sedangkan area lain diringkas menjadi empat subbab (keputusan ikpu 2026-11-03). Perlu dicek ulang setelah BAB IV ditulis.
6. **Selesai (2026-11-05):** `00-Admin/01-TOR.md` dilengkapi dari TOR asli. Susunan dokumen belum memuat beberapa keluaran TOR: batas peran SoR–MDM–ESDP–Cloudera, rincian *end-to-end* UC1 dan UC3, SLA/rekonsiliasi/*onboarding*, arsitektur transisi dan target kematangan, rekomendasi teknologi dan faktor pendorong investasi, serta "Dokumen Hasil Pengujian dan Deployment". Perlu diputuskan apakah ditambah sebagai subbab baru (rincian di `00-Admin/01-TOR.md` bagian 9).
