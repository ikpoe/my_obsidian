---
title: "BAB IX PENUTUP"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB IX PENUTUP

Bab ini menyajikan kesimpulan kajian beserta rekomendasi kebijakan bagi pimpinan.

## 9.1 Kesimpulan

Kajian ini menunjukkan bahwa AI dan *dashboard* yang andal membutuhkan tata kelola data dari ujung ke ujung. Kondisi saat ini memperlihatkan skala teknis yang sudah besar, tetapi kendali tata kelolanya belum sebanding dengan skala tersebut. Berdasarkan hal tersebut, arah yang dituju adalah pembagian peran yang ringkas dan jelas, klasifikasi yang berjalan, metrik yang dapat dipercaya, serta pemanfaatan AI yang baru dijalankan setelah *use case*-nya memenuhi kriteria kelayakan. Arsitektur ditetapkan sebagai kemampuan yang netral terhadap merek layanan, dengan rincian layanan awan yang disusun kemudian. Sebagai bekal pelaksanaan, kajian ini menyediakan daftar SOP yang wajib disusun, kriteria pemilihan domain pilot, dan kerangka penilaian kematangan untuk dijalankan pada *Checkpoint* 0.

## 9.2 Rekomendasi Kebijakan

Berdasarkan kesimpulan tersebut, kajian ini merekomendasikan kepada pimpinan langkah-langkah kebijakan berikut, yang dimulai pada *Checkpoint* 0.

1.  **Penetapan arah kebijakan.** Pimpinan menetapkan arah "fondasi tata kelola sebelum AI diperluas" dengan urutan prioritas: domain pilot, klasifikasi, dan RACI; kemudian register dan *lineage* tingkat 1; kemudian rilis angka resmi; dan baru setelah itu pemanfaatan AI/BI.

2.  **Penetapan domain pilot.** Domain pilot ditetapkan pada *Checkpoint* 0 berdasarkan empat kriteria, yakni dampak layanan, kesiapan data dan pemilik data, risiko privasi, serta hasil yang terukur dalam 3–6 bulan.

3.  **Penunjukan pemilik data dan *steward*.** Pemilik data dan *steward* untuk domain pilot ditunjuk melalui SK atau surat penunjukan, dengan pembagian peran RACI ringkas sebagai acuan.

4.  **Penetapan bentuk instrumen.** Pimpinan menetapkan bentuk instrumen kebijakan internal manajemen data dan klasifikasi, berupa SK atau pedoman internal.

5.  **Penugasan penyusunan SOP gelombang 1.** SOP gelombang 1 mencakup penetapan dan peninjauan klasifikasi data, pengajuan dan persetujuan akses data, pengisian dan pemeliharaan register, rilis angka resmi, penanganan masalah kualitas data, serta pengajuan *use case* AI/BI, beserta unit penanggung jawabnya.

6.  **Penerapan kriteria kelayakan AI.** Kriteria kelayakan AI menjadi syarat wajib bagi setiap usulan *use case* AI/BI baru.

7.  **Pelaksanaan penilaian kematangan.** Penilaian kematangan dilaksanakan pada *Checkpoint* 0 berbasis bukti, melalui wawancara atau lokakarya dengan DITTIK, pengelola data, dan unit keamanan informasi, lalu dikalibrasi bersama. Profil yang dihasilkan dipakai untuk menetapkan prioritas *checkpoint* berikutnya.

8.  **Pengisian register awal dan pelaksanaan *Checkpoint* 1–2.** Setelah kedua *checkpoint* tersebut terpenuhi, AI/BI baru diperluas secara terbatas melalui kriteria kelayakan *use case*.

9.  **Pengukuran ulang skala data.** Ukuran penyimpanan analitik, jumlah koneksi sumber, jumlah proses ETL, dan jumlah *job* harian diukur ulang, terutama apabila angka tersebut dibutuhkan untuk keputusan kapasitas.
