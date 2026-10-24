---
title: "LAMPIRAN A CHECKLIST KLASIFIKASI DATA"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# LAMPIRAN A *CHECKLIST* KLASIFIKASI DATA

Lampiran ini memuat usulan kerja, sedangkan keputusan final berada pada pemilik data dan kebijakan keamanan instansi.

### A.1 Cara pakai

*Checklist* ini digunakan untuk mengusulkan kelas setiap objek data, baik berupa set data, tabel, metrik, maupun data yang dikirim ke GenAI.

### A.2 Dasar dan asumsi

Usulan klasifikasi dalam lampiran ini disusun berdasarkan sejumlah ketentuan dan asumsi kerja berikut.

- Peraturan BSSN 4/2021 menjaga kerahasiaan antara lain melalui penetapan klasifikasi informasi, enkripsi, dan pembatasan akses. Peraturan ini tidak menetapkan nama tingkat kelas, sehingga nama kelas di bawah adalah usulan.

- UU 27/2022 (PDP) menjadi dimensi silang yang membedakan bukan data pribadi, data pribadi umum, dan data pribadi spesifik.

- Perpres 39/2019 dan 95/2018 memberi konteks Satu Data dan SPBE.

- Asumsi kerja: (A1) belum ada kebijakan klasifikasi formal di tingkat direktorat; (A2) prioritasnya adalah data untuk pelaporan dan sistem sumbernya; (A3) kelas yang diusulkan adalah Publik / Internal / Terbatas / Rahasia, dan dapat diganti dengan istilah resmi; (A4) angka resmi tidak sama dengan keluaran GenAI mentah, sehingga perlu peninjauan manusia; (A5) daftar sistem yang tersedia hanya gambaran, bukan kamus data lengkap.

### A.3 Kelas dan pemakaian GenAI

Kelas yang diusulkan adalah Publik, Internal, Terbatas, dan Rahasia, ditambah label PDP pada setiap objek. Data Publik dan data Internal yang sudah diagregasi boleh dipakai GenAI secara terkendali. Secara bawaan, data Terbatas tidak dikirim ke model publik, sedangkan data Rahasia sama sekali tidak boleh dikirim ke model publik.

### A.4 Pertanyaan per objek

Setiap objek terlebih dahulu diidentifikasi menurut nama, sistem sumber, usulan pemilik, usulan *steward*, dan pemakaiannya dalam laporan/*dashboard*. Selanjutnya, kelas ditentukan dengan pertanyaan berikut.

1.  Apakah data sudah dipublikasikan secara resmi? Jika ya, kelasnya Publik.

2.  Apakah data memuat identitas orang atau biometrik? Jika ya, kelasnya minimal Terbatas; biometrik cenderung berkelas Rahasia dan termasuk data pribadi spesifik.

3.  Apakah data menyangkut penegakan hukum yang belum dipublikasikan? Jika ya, kelasnya Terbatas sampai Rahasia.

4.  Apakah data berupa agregat yang secara wajar tidak dapat dipakai untuk mengenali individu? Jika ya, kelasnya Internal atau Publik.

5.  Apakah kebocoran data dapat mengganggu operasi perbatasan atau keselamatan? Jika ya, kelasnya dinaikkan.

6.  Apakah data merupakan gabungan beberapa data yang dapat mengenali individu? Jika ya, kelasnya dinaikkan.

7.  Apakah data memuat rincian penerimaan negara bukan pajak (PNBP) per individu? Jika ya, kelasnya minimal Terbatas.

8.  Apakah data hanya berupa metadata teknis? Jika ya, kelasnya biasanya Internal.

Hasil keputusan dicatat, meliputi kelas, label PDP, retensi/*masking*, aturan berbagi, izin GenAI, pemakaian di *dashboard* eksekutif, dan pihak yang menyetujui.

### A.5 Contoh usulan kelas pada rantai pelaporan

Usulan kelas berikut masih bersifat awal. Data transaksi rinci yang memuat data pribadi selalu berkelas lebih tinggi daripada agregatnya.

| No | Tema laporan | Usulan kelas | GenAI |
|----|----|----|----|
| 1 | Penerbitan dokumen perjalanan | Internal (rincian: Terbatas) |  |
| 2 | Izin tinggal | Internal/Terbatas |  |
| 3 | Izin tinggal dan penjamin | Terbatas |  |
| 4 | Verifikasi izin tinggal (tabel data pribadi) | Terbatas–Rahasia | Tidak |
| 5 | Visa | Internal/Terbatas |  |
| 6 | Perlintasan | Internal/Terbatas |  |
| 7 | Penyidikan | Rahasia | Tidak |
| 8 | Potensi *overstay* | Terbatas–Rahasia | Tidak |
| 9 | Visa perwakilan | Internal/Terbatas |  |
| 10 | Deportasi/pemulangan | Rahasia/Terbatas | Tidak |
| 11 | Pendetensian | Rahasia | Tidak |
| 12 | Monitoring nasional | Internal/Terbatas |  |
| 13 | Diaspora | Terbatas |  |
| 14 | *Overstay* yang sudah dibayar | Terbatas |  |
| 15 | Kepatuhan ITAS/ITAP | Terbatas |  |
| 16 | Bukti kecukupan dana | Rahasia/Terbatas | Tidak |

Sebagai contoh pada sistem sumber, data paspor/dokumen perjalanan berkelas Terbatas–Rahasia, dan data cegah-tangkal dan biometrik berkelas Rahasia. Penyimpanan analitik dan gabungan data mengikuti kelas tertinggi dari sumbernya, dan penurunan kelas hanya dapat dilakukan melalui proses formal. Salinan di lokasi DR mengikuti kelas yang sama dengan data aslinya.

### A.6 *Guardrail* AI/BI

Pemakaian AI/BI atas data yang telah diklasifikasi mengikuti *guardrail* minimum berikut.

- GenAI hanya memakai objek yang diizinkan *steward*.

- Secara bawaan dilarang dikirim ke model publik: data pribadi, biometrik, data penegakan hukum, bukti kecukupan dana, dan daftar individu *overstay*.

- Angka resmi hanya dari metrik tersertifikasi dan ditinjau manusia.

- Pemakaian AI dicatat.

### A.7 Langkah operasional

*Checklist* ini dijalankan melalui langkah operasional berikut secara berurutan.

1.  *Steward* mengisi *checklist* untuk objek prioritas sepanjang rantai pelaporan.

2.  Pemilik data dan unit keamanan informasi meninjau.

3.  Hasilnya dimasukkan ke register klasifikasi.

4.  Hak akses disesuaikan dengan kelas yang telah ditetapkan.

5.  Setelah itu, AI terbatas dapat diaktifkan.
