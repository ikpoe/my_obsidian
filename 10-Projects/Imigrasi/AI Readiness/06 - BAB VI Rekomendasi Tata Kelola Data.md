---
title: "BAB VI REKOMENDASI TATA KELOLA DATA"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB VI REKOMENDASI TATA KELOLA DATA

Arsitektur data yang dituju hanya dapat berjalan apabila didukung oleh kebijakan, pembagian peran, dan cara kerja minimum di tingkat organisasi. Rekomendasi dalam bab ini mencakup ketiga hal tersebut, dengan pembagian peran yang dirumuskan secara ringkas dan SOP rinci yang disusun kemudian berdasarkan masukan unit terkait. Rekomendasi tersebut bertujuan agar setiap data dan angka laporan memiliki pihak yang bertanggung jawab, klasifikasi digunakan sebagai dasar pemberian akses, dan register mulai diisi. Selain itu, kualitas dan *lineage* perlu memadai untuk angka penting, sementara AI/BI tidak digunakan secara bebas tanpa syarat.

## 6.1 Kerangka Tata Kelola Data

Tata kelola data pada dasarnya mengatur empat keputusan, yaitu siapa pemilik data dan siapa yang mengurusnya sehari-hari, kelas setiap data dan pihak yang berhak mengaksesnya, definisi angka resmi dan pihak yang berwenang mengubahnya, serta cara data dipakai untuk laporan, berbagi data, dan AI. Keempat keputusan tersebut memerlukan pemegang kewenangan yang jelas agar tidak diputuskan kasus per kasus.

Kerangka minimum yang direkomendasikan terdiri atas kebijakan internal manajemen data yang ringkas dan dapat diterapkan, RACI ringkas untuk domain laporan prioritas, serta register objek data dan metrik penting. Kerangka tersebut dilengkapi aturan klasifikasi dan akses, aturan kualitas dan rilis angka, serta aturan pemakaian AI/BI melalui kriteria kelayakan *use case* AI. Koordinasi dijalankan melalui forum yang sederhana tanpa harus menunggu terbentuknya forum besar yang baru.

## 6.2 Pembagian Peran (RACI Ringkas)

Istilah peran yang digunakan sejalan dengan Satu Data dan tidak menggantikan forum tingkat kementerian. Pemilik data berperan sebagai Accountable yang berwenang atas definisi dan pemakaian angka atau domain data, sedangkan *steward* berperan sebagai Responsible yang mengurus kualitas, metadata, dan masalah harian. Pengelola platform teknis mengurus *pipeline*, akses teknis, dan infrastruktur data, dan peran ini pada umumnya melekat pada DITTIK. Pemakai, baik untuk laporan, analisis, maupun AI, memakai data sesuai dengan kelas dan tujuannya, sementara pengawas kepatuhan dan keamanan informasi menetapkan batas PDP dan keamanan.

Contoh RACI untuk domain laporan prioritas disajikan pada tabel berikut. Nama unit pengisi setiap peran ditetapkan bersama unit terkait pada domain pilot.

| Kegiatan | Pemilik data | *Steward* | Platform teknis | Pemakai | Kepatuhan/ keamanan |
|----|----|----|----|----|----|
| Menetapkan definisi metrik resmi | A | R | C | C | C |
| Mengisi dan menjaga register serta metadata | A | R | C | I | C |
| Menjalankan *pipeline* dan akses teknis | C | C | R/A\* | I | C |
| Menetapkan kelas data dan hak akses | A | R | C | I | R/C |
| Merilis angka untuk pimpinan | A | R | C | C | C |
| Menyetujui *use case* AI yang memakai data | A | C | C | R (pengusul) | R |

Keterangan: tanda \* menunjukkan bahwa peran "A" pada kolom platform teknis hanya berlaku untuk operasional teknis, sedangkan tanggung jawab akhir atas makna angka tetap berada pada pemilik data. R = mengerjakan; A = bertanggung jawab akhir; C = dimintai pendapat; I = diberi tahu. Templat ini bersifat awal dan akan diperdalam setelah diuji pada domain pilot. Ringkasan pembagian peran ditunjukkan pada Gambar 3.

![[Gambar 3 - Peran dan RACI Ringkas.png]]

*Gambar 3. Peran Tata Kelola Data dan RACI Ringkas (Templat). Sumber: hasil olahan kajian.*

## 6.3 Hubungan dengan Forum Satu Data

Forum Satu Data tingkat kementerian tetap menjadi wadah resmi lintas unit apabila telah berjalan sehingga pembentukan forum tandingan dapat dihindari. Di tingkat Direktorat Jenderal Imigrasi, setiap domain prioritas memerlukan titik kontak pemilik data dan *steward* yang jelas. Koordinasi dijalankan melalui rapat singkat berkala, misalnya bulanan, untuk membahas kualitas, definisi, dan akses data. Selain itu, jalur eskalasi ke pejabat berwenang perlu tersedia apabila terdapat angka yang saling bertentangan atau permintaan akses yang menyangkut data sensitif.

## 6.4 Kebijakan Internal Manajemen Data (Isi Minimum)

Kebijakan internal manajemen data sekurang-kurangnya memuat hal-hal berikut.

1.  tujuan dan ruang lingkup (rantai laporan dan analitik);

2.  peran (merujuk RACI);

3.  klasifikasi data dan aturan akses;

4.  standar metadata dan register minimum;

5.  kualitas data dan proses rilis angka resmi;

6.  berbagi data dengan pihak internal dan eksternal;

7.  pemakaian data untuk AI/BI;

8.  pencatatan dan audit sederhana;

9.  peninjauan kebijakan secara berkala.

Kebijakan tersebut dapat berbentuk surat keputusan atau pedoman internal sesuai dengan praktik hukum yang berlaku. Bentuk instrumen tersebut ditetapkan oleh pimpinan.

## 6.5 Klasifikasi Data dan Akses

Klasifikasi data menggunakan skema kelas yang disepakati, dengan usulan awal empat kelas, yaitu Publik, Internal, Terbatas, dan Rahasia, ditambah label PDP pada setiap objek. Setiap objek dalam register prioritas wajib memiliki kelas, dan akses diberikan sesuai dengan kelas data tersebut, bukan diberikan secara luas atas dasar kebiasaan. Pemrosesan data pribadi mengikuti UU PDP sehingga memiliki tujuan yang jelas, menggunakan data seminimal mungkin, dan memiliki dasar pemrosesan yang sah. Untuk pemanfaatan AI/BI, data kelas tinggi tidak boleh digunakan dalam uji coba bebas tanpa izin pemilik data dan catatan kepatuhan.

## 6.6 Kualitas Data dan Rilis Angka

Angka yang dipakai pimpinan atau dimuat dalam laporan resmi harus memiliki definisi tertulis mengenai hal yang dihitung, periode, dan filternya. Sebelum dirilis, angka tersebut melalui pemeriksaan sederhana atas kelengkapan, kejanggalan dibandingkan dengan periode sebelumnya, dan sumber datanya. Setiap koreksi angka dicatat, termasuk pihak yang mengubah dan alasannya. Dalam pelaksanaannya, *steward* memimpin pemeriksaan harian atau berkala, sedangkan pemilik data mengesahkan angka resmi. *Pipeline* yang gagal atau terlambat juga harus segera terlihat melalui peringatan sehingga gangguan tidak baru diketahui ketika *dashboard* kosong.

## 6.7 Katalog, Metadata, dan *Lineage*

Katalog, metadata, dan *lineage* sekurang-kurangnya memuat daftar sistem sumber penting beserta koneksi utamanya, daftar objek data, tabel, atau berkas prioritas, serta daftar metrik resmi prioritas. Untuk metrik kritis, katalog juga memuat keterangan mengenai sumber utama, olahan utama, dan pemakai utama sebagai *lineage* tingkat 1. Isi katalog tersebut disempurnakan secara bertahap seiring dengan bertambahnya domain. Keberadaan katalog memudahkan pemakai menelusuri asal suatu angka tanpa harus terus bertanya dan sekaligus mencegah pembuatan salinan data liar tanpa catatan.

## 6.8 Siklus Hidup Data dan Produk Data Sederhana

Setiap objek prioritas idealnya memiliki keterangan asal dan pemilik, jadwal pembaruan, retensi, serta status uji coba, resmi, atau dihentikan. Kumpulan data beserta definisinya yang siap dipakai untuk laporan dapat dikelola sebagai produk data sederhana, tetapi hal yang terpenting adalah pemisahan antara versi yang resmi dipakai dan area uji coba.

## 6.9 Etika Pemakaian Data dan Batas AI/BI

Pemanfaatan AI/BI berfungsi membantu, sedangkan keputusan resmi tetap berada pada pejabat atau proses yang berwenang. Keluaran AI tidak dapat dipakai sebagai angka resmi sebelum melewati proses rilis metrik. Setiap *use case* AI baru harus memenuhi kriteria kelayakan yang diperiksa secara singkat, meliputi tujuan, data yang dipakai, kelas data, risiko PDP, pemilik, dan cara evaluasi. Usulan yang belum memenuhi kriteria kelayakan dimasukkan ke *backlog* dan tidak langsung masuk ke *production*. Penyalahgunaan akses dan kebocoran data melalui kanal *chat* dan AI juga perlu dipantau dengan aturan rinci yang disusun kemudian.

## 6.10 Instrumen Penetapan

Agar rekomendasi tidak berhenti sebagai wacana, sejumlah instrumen penetapan direkomendasikan untuk disiapkan secara bertahap. Instrumen tersebut meliputi pedoman atau kebijakan internal manajemen data dan klasifikasi, SK atau surat penunjukan pemilik data dan *steward* domain prioritas sebagai wujud RACI ringkas, serta templat register metrik dan register objek. Instrumen lain yang diperlukan adalah *checklist* rilis angka dan *checklist* kriteria kelayakan *use case* AI, serta SOP per proses yang lebih rinci.

## 6.11 Daftar SOP yang Perlu Dibuat

Daftar SOP berikut disediakan agar unit terkait mengetahui dokumen operasional yang perlu disusun. Isi setiap SOP disusun kemudian oleh unit yang bersangkutan.

| Kelompok | SOP |
|---|---|
| A. Data dan klasifikasi | 1. Penetapan dan peninjauan klasifikasi data<br>2. Pengajuan dan persetujuan akses data<br>3. Berbagi data ke pihak internal<br>4. Berbagi data ke pihak eksternal/permintaan data |
| B. Register, metadata, dan angka resmi | 5. Pengisian dan pemeliharaan register objek data/metrik<br>6. Perubahan definisi metrik resmi<br>7. Rilis angka untuk pimpinan/laporan resmi<br>8. Koreksi angka setelah rilis |
| C. Kualitas dan *pipeline* | 9. Penanganan masalah kualitas data<br>10. Pemantauan *job* data (gagal, terlambat, anomali volume)<br>11. Eskalasi bila angka atau *pipeline* bermasalah |
| D. AI/BI | 12. Pengajuan *use case* AI/BI (kriteria kelayakan AI)<br>13. Pemakaian data untuk uji coba AI/BI<br>14. Kenaikan *use case* AI/BI dari uji coba ke pemakaian terbatas |
| E. Peran dan perubahan | 15. Pembekalan awal pemilik data/*steward* (hak dan kewajiban singkat)<br>16. Serah terima peran saat personel berganti<br>17. Perubahan kebijakan internal manajemen data |

**Prioritas penulisan (usulan):** SOP gelombang 1 mencakup SOP penetapan dan peninjauan klasifikasi data, pengajuan dan persetujuan akses data, pengisian dan pemeliharaan register, rilis angka, penanganan masalah kualitas data, serta pengajuan *use case* AI/BI. Gelombang 2 mencakup SOP lainnya sesuai dengan kebutuhan domain prioritas.

Isi SOP disusun oleh unit fungsi terkait, yaitu unit teknis, pengelolaan data, dan kepatuhan, dengan menggunakan templat bersama.
