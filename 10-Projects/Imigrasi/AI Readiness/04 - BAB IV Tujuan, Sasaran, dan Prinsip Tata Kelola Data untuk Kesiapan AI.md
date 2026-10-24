---
title: "BAB IV TUJUAN, SASARAN, DAN PRINSIP TATA KELOLA DATA UNTUK KESIAPAN AI"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB IV TUJUAN, SASARAN, DAN PRINSIP TATA KELOLA DATA UNTUK KESIAPAN AI

Bab ini merumuskan kondisi yang ingin dicapai melalui tata kelola data, yang dijabarkan ke dalam visi, tujuan, sasaran, pilar, prinsip, dan urutan prioritas. Kondisi tersebut bukan sekadar bertambahnya jumlah *dashboard* atau *use case* AI, melainkan tata kelola data yang membuat angka laporan dapat dipercaya, akses data terkendali, dan fondasi data siap untuk analitik serta AI yang bertanggung jawab. Untuk mencapai kondisi tersebut, urutan langkah yang ditempuh diawali dengan menaikkan kematangan tata kelola dan kualitas data, dilanjutkan dengan penataan pemakaian laporan, dan baru kemudian diikuti perluasan AI/BI. Urutan ini menempatkan fondasi data sebagai syarat, bukan sebagai pelengkap, bagi setiap inisiatif AI.

## 4.1 Visi Tata Kelola Data

Direktorat Jenderal Imigrasi mengelola data keimigrasian sebagai aset strategis yang terklasifikasi, dapat ditelusuri, dan layak digunakan untuk keputusan operasional maupun kebijakan, termasuk untuk analitik dan AI. Pengelolaan tersebut dijalankan dengan tetap mematuhi ketentuan SPBE, Satu Data, PDP, dan keamanan informasi.

## 4.2 Tujuan

Tujuan tata kelola data disusun untuk mewujudkan visi tersebut. Tujuan tersebut adalah sebagai berikut.

1.  Mewujudkan tata kelola data ujung ke ujung pada rantai pelaporan prioritas (mulai dari sumber, pengolahan, dan metrik hingga pemakaian).

2.  Menetapkan dan menjalankan klasifikasi informasi serta pengendalian akses yang konsisten, termasuk untuk kanal AI/BI.

3.  Memastikan angka resmi dan indikator utama memiliki definisi, pemilik, serta catatan kualitas dan *lineage* yang memadai.

4.  Menyelaraskan investasi platform data, analitik, dan AI agar mendukung fondasi di atas, dengan kebijakan yang konseptual dan netral terhadap penyedia.

5.  Menyiapkan *guardrail* AI/BI agar eksplorasi AI generatif (GenAI) dan BI tidak menghasilkan keputusan yang tidak dapat dipertanggungjawabkan.

## 4.3 Sasaran dan Indikator Keberhasilan

Sasaran disusun sebagai hasil tata kelola yang dapat diperiksa pada setiap *checkpoint*, bukan sebagai jadwal bertanggal yang ketat. Indikator pada tabel berikut masih merupakan usulan awal yang perlu dikalibrasi bersama unit terkait.

| Kode | Sasaran | Contoh indikator |
|----|----|----|
| S1: Peran jelas | Domain pelaporan prioritas memiliki pemilik data dan *steward* yang ditunjuk sesuai RACI. | Persentase domain prioritas yang sudah memiliki RACI ringkas; tersedia jalur eskalasi kualitas dan akses. |
| S2: Klasifikasi berjalan | Skema klasifikasi disepakati dan diterapkan pada objek data prioritas serta aturan akses dasar. | Pedoman klasifikasi internal terbit; register objek prioritas berisi kelas dan dasar akses; tinjauan berkala terjadwal. |
| S3: Register, metadata, *lineage* | Objek data dan metrik utama terdaftar; jalur dari sumber ke pemakai dapat dijelaskan untuk indikator kritis. | Register minimum aktif untuk sejumlah objek prioritas; *lineage* tingkat 1 untuk indikator kritis. |
| S4: Kualitas dan rilis angka | Ada aturan kualitas minimum dan proses rilis untuk angka resmi yang dipakai pimpinan. | Daftar metrik tersertifikasi; *checklist* rilis (kelengkapan, anomali, sumber) sebelum publikasi internal. |
| S5: Pemakaian laporan terkendali | *Dashboard*/laporan prioritas merujuk metrik tersertifikasi; permintaan data eksternal mencatat kelas data dan tujuan. | Peta keterkaitan *dashboard* prioritas dengan metrik/sumber; SOP permintaan data memuat cek klasifikasi/PDP. |
| S6: AI di atas fondasi | *Use case* AI/BI hanya masuk ke *production* terbatas jika syarat data (izin, kelas, kualitas, audit) terpenuhi. | *Checklist* kriteria kelayakan AI dipakai; *backlog* terpisah dari daftar *use case* yang disetujui. |

## 4.4 Pilar Pengembangan

Pilar pengembangan mengikuti semangat P3GC dengan menempatkan tata kelola sebagai pengikat, tanpa menjadikan pembentukan tim AI sebagai pusat pengembangan. Pilar Orang mencakup peran pemilik data dan *steward*, literasi data dasar, serta alih pengetahuan dari mitra. Pilar Proses meliputi klasifikasi, register, rilis angka, permintaan data, dan pengendalian perubahan metrik maupun *pipeline*. Pilar Platform mencakup katalog dan metadata, pemantauan kualitas, pemisahan OLTP dan analitik, serta kendali akses. Pilar Tata Kelola meliputi kebijakan internal, RACI ringkas, kepatuhan terhadap PDP dan SPBE, serta *guardrail* AI. Pilar Perubahan mencakup komunikasi perubahan angka atau definisi, penerapan *checkpoint*, dan pencegahan beredarnya metrik tidak resmi secara paralel.

## 4.5 Prinsip Dasar Pengelolaan Data

Pengelolaan data berpegang pada tujuh prinsip dasar yang saling melengkapi. Prinsip tersebut menjadi acuan dalam menilai setiap usulan kebijakan, arsitektur, maupun pemanfaatan AI sebagaimana diuraikan berikut.

1.  **P1: Fondasi sebelum AI diperluas.** GenAI/BI tidak diperluas sebelum klasifikasi, kepemilikan, dan kualitas minimum terpenuhi.

2.  **P2: Satu definisi untuk angka resmi.** Metrik kritis memiliki pemilik, definisi, dan versi, serta setiap perubahannya dicatat.

3.  **P3: Klasifikasi menentukan akses dan pemakaian.** Kelas data mengatur siapa yang boleh melihat, mengolah, dan memakainya untuk AI.

4.  **P4: *Lineage* memadai untuk dipercaya.** Untuk indikator kritis, sumber dan olahan utama dapat dijelaskan tanpa penelusuran manual yang panjang.

5.  **P5: Minimisasi dan tujuan terbatas (PDP).** Data sensitif tidak mengalir ke kanal eksplorasi tanpa dasar dan kendali.

6.  **P6: Kebijakan netral merek.** Arsitektur ditetapkan pada tingkat kemampuan, sedangkan pemilihan produk awan atau alat merupakan keputusan implementasi.

7.  **P7: Dijalankan, bukan sekadar ditulis.** Setiap kebijakan memiliki register, peran, dan jadwal peninjauan.

## 4.6 Urutan Prioritas

Prioritas pelaksanaan disusun sebagai urutan logis dan bukan sebagai jadwal kalender. Langkah pertama adalah penetapan domain pilot, penyepakatan skema klasifikasi, dan penetapan RACI ringkas untuk domain tersebut. Langkah berikutnya adalah pembangunan register objek dan metrik prioritas beserta *lineage* tingkat 1, yang kemudian diikuti penataan rilis angka dan peta pemakaian laporan prioritas. Setelah fondasi tersebut terbentuk, *guardrail AI/BI dan kriteria kelayakan AI diterapkan* bagi *use case* baru. Pendalaman platform, seperti katalog dan pemantauan kualitas otomatis, dilakukan kemudian sesuai dengan celah yang telah teridentifikasi.

## 4.7 Keselarasan dengan Sasaran Strategis dan IKU

Sasaran S1 sampai dengan S6 pada prinsipnya mendukung pelayanan, pengawasan, dan akuntabilitas berbasis data. Pemetaan ke IKU dan Renstra dilakukan setelah dokumen Renstra beserta turunan indikator resminya tersedia. Sementara itu, keberhasilan kajian diukur dari tercapainya sasaran tersebut pada setiap *checkpoint* pelaksanaan.
