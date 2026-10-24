---
title: "BAB VIII PEMANFAATAN ANALITIK DAN AI, PENILAIAN KEMATANGAN, CHECKPOINT IMPLEMENTASI, DAN MANAJEMEN RISIKO"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB VIII PEMANFAATAN ANALITIK DAN AI, PENILAIAN KEMATANGAN, *CHECKPOINT* IMPLEMENTASI, DAN MANAJEMEN RISIKO

Pemanfaatan analitik dan AI di lingkungan Direktorat Jenderal Imigrasi perlu berdiri di atas fondasi tata kelola data yang memadai. Bab ini membahas cara data dimanfaatkan untuk laporan, analitik, dan AI di atas fondasi tersebut, pedoman bagi pimpinan dalam memilih domain pilot, kerangka penilaian kematangan, serta *checkpoint* pelaksanaan yang menetapkan hal yang harus diselesaikan lebih dahulu tanpa mengunci tanggal kalender. Selain itu, bab ini menguraikan risiko utama beserta cara penanganannya. Pada prinsipnya, AI dan *dashboard* yang lebih cerdas dapat dikembangkan, tetapi tidak boleh mendahului klasifikasi, peran, register, dan rilis angka.

## 8.1 Kriteria Prioritas Pemanfaatan

Usulan *use case*, baik untuk laporan, analitik, maupun AI, diprioritaskan apabila pemilik datanya jelas, sumber datanya dapat dijelaskan, dan kelas datanya telah ditetapkan. Usulan tersebut juga harus memiliki manfaat yang jelas bagi pelayanan, pengawasan, atau kebijakan, dengan risiko PDP dan keamanan yang dapat dikelola. Keberhasilannya pun harus dapat diukur dengan metrik sederhana. Apabila salah satu syarat tersebut belum terpenuhi, usulan dimasukkan ke dalam *backlog* dan tidak langsung masuk ke *production*.

## 8.2 Pedoman Pemilihan Domain Pilot

Peran, klasifikasi, register, dan rilis angka perlu diuji lebih dahulu pada satu domain pelaporan, yaitu domain pilot, sebelum diperluas ke domain lain. Pemilihan domain pilot merupakan kewenangan pimpinan dengan menggunakan empat kriteria sebagaimana tercantum pada tabel berikut.

| **Kriteria** | **Pertanyaan kunci** | **Tanda domain layak dipilih** |
|----|----|----|
| 1\. Dampak layanan | Apakah perbaikan data di domain ini langsung terasa pada pelayanan, pengawasan, atau keputusan pimpinan? | Angkanya dipakai rutin oleh pimpinan atau memengaruhi proses layanan. |
| 2\. Kesiapan data dan pemilik data | Apakah sumber data utamanya diketahui dan ada pejabat yang bersedia menjadi pemilik data serta menunjuk *steward*? | Sumber dan pemakai utama dapat disebut; calon pemilik data dan *steward* tersedia. |
| 3\. Risiko privasi | Seberapa sensitif data pribadi atau data penegakan hukum yang terlibat? | Risiko dapat dikelola dengan kendali dasar; pilot pertama sebaiknya tidak bergantung pada data kelas Rahasia. |
| 4\. Terukur dalam 3–6 bulan | Apakah hasilnya dapat ditunjukkan dalam 3–6 bulan dengan indikator sederhana? | Ada 2–3 metrik yang dapat disertifikasi dan dibandingkan sebelum dan sesudah pilot. |

Dalam penerapannya, setiap calon domain dinilai secara sederhana, misalnya dengan skala tinggi, sedang, atau rendah, pada keempat kriteria tersebut, kemudian hasilnya dibahas bersama calon pemilik data. Berdasarkan hasil pembahasan itu, pimpinan menetapkan domain pilot pada *Checkpoint* 0.

Data potensi *overstay* dapat dijadikan ilustrasi cara menilai calon domain tanpa dimaksudkan sebagai pilihan domain pilot. Dampak layanannya tinggi karena terkait dengan proses keberangkatan, tetapi risiko privasinya juga tinggi karena memuat data individu.

## 8.3 Pemanfaatan untuk Pelaporan dan Analitik (Jangka Dekat)

Dalam jangka dekat, pemanfaatan data untuk pelaporan dan analitik diarahkan pada penataan angka resmi untuk memantau kinerja layanan melalui metrik tersertifikasi. *Dashboard* dan laporan prioritas dipetakan ke sumber dan definisi yang sama, dan layanan permintaan data eksternal diperbaiki agar memuat pemeriksaan kelas data dan tujuan. Celah hasil olahan yang tidak kembali ke operasional ditutup dengan menetapkan pemilik proses yang jelas. Salah satu contohnya adalah temuan *overstay* yang belum dimanfaatkan pada proses keberangkatan.

## 8.4 Pemanfaatan AI/BI di Atas Fondasi

Pemanfaatan AI/BI dalam jangka dekat diprioritaskan untuk membantu penyusunan dan eksplorasi laporan serta tanya-jawab atas data yang telah memenuhi kriteria kelayakan. Gagasan lain seperti prediksi volume layanan, deteksi anomali lanjutan, dan asisten dokumen tetap berada dalam *backlog* karena belum memiliki fondasi data atau izin yang memadai. Dalam pemanfaatannya, keluaran AI tidak menjadi angka resmi sebelum melewati proses rilis metrik, dan data sensitif tidak digunakan dalam uji coba bebas. Setiap *use case* AI juga wajib mengisi *checklist kriteria kelayakan dan mengikuti SOP terkait.* Adapun desain tim platform AI dan perangkat teknis yang rinci berada di luar cakupan kajian ini. Pengaturan dalam bab ini terbatas pada batas tata kelolanya.

## 8.5 Kerangka Penilaian Kematangan yang Diusulkan

Kerangka penilaian kematangan berikut diusulkan sebagai alat untuk mengukur kesiapan tata kelola data. Penilaian yang sebenarnya dijalankan sebagai tindak lanjut pada *Checkpoint* 0. Kerangka tersebut diuraikan ke dalam empat unsur, yaitu rujukan, dimensi, cara menilai, dan keluaran.

**1. Rujukan.** Dimensi penilaian merujuk pada *knowledge area* manajemen data dalam DAMA-DMBOK terbitan DAMA International (2017), antara lain tata kelola data, kualitas data, metadata, keamanan data, serta data induk dan data referensi. Tingkat penilaian diselaraskan dengan lima tingkat kapabilitas SPBE dalam Peraturan Menteri Pendayagunaan Aparatur Negara dan Reformasi Birokrasi (PermenPANRB) Nomor 59 Tahun 2020, yaitu Rintisan, Terkelola, Terdefinisi, Terpadu dan Terukur, serta Optimum. Penyelarasan tersebut bertujuan untuk memudahkan penghubungan hasil penilaian dengan Indeks SPBE. Data Management Capability Assessment Model (DCAM) versi 3 dari EDM Council (2025) dan Data Management Maturity (DMM) dari CMMI Institute (2014) dapat digunakan sebagai pembanding, tetapi tidak dijadikan acuan utama. Untuk konteks sektor publik, Central Digital and Data Office (2023) menyusun Data Maturity Assessment for Government yang menilai kematangan data pada sepuluh topik dan enam tema dengan lima tingkat kematangan, sehingga kerangka tersebut juga relevan sebagai pembanding bagi instansi pemerintah.

**2. Dimensi.** Penilaian menggunakan lima dimensi, yaitu Orang, Proses, Data, Platform, dan Kesiapan AI/BI, dengan tata kelola sebagai pengikat kelima dimensi tersebut. Uraian setiap tingkat pada masing-masing dimensi disajikan pada tabel berikut dan tidak dimaksudkan sebagai skor.

| **Dimensi** | **1. Rintisan** | **2. Terkelola** | **3. Terdefinisi** | **4. Terpadu dan Terukur** | **5. Optimum** |
|----|----|----|----|----|----|
| **Orang** | Tidak ada pemilik data/*steward* ditunjuk | Peran informal, sangat bergantung pada mitra | Pemilik data dan *steward* ditetapkan lewat SK untuk domain prioritas | Peran dievaluasi berkala; alih pengetahuan dari mitra terdokumentasi | Peran dan kompetensi ditinjau dan disempurnakan secara berkelanjutan di semua domain |
| **Proses** | Rilis angka dan akses data kasus per kasus | Sebagian praktik ada, belum tertulis | SOP gelombang 1 berlaku dan dipakai | Kepatuhan SOP dipantau dan diperbaiki dari temuan | SOP disempurnakan secara berkelanjutan dari hasil evaluasi; praktik baik diterapkan lintas domain |
| **Data** | Belum ada register maupun klasifikasi | Inventori parsial; klasifikasi sebatas usulan | Objek prioritas terdaftar, punya kelas, dan *lineage* tingkat 1 | Kualitas diukur dengan aturan tertentu; tren dilaporkan | Register, kelas, dan kualitas data dikelola lintas domain dan diperbaiki berdasarkan tren |
| **Platform** | *Pipeline* tanpa pemantauan | Pemantauan dasar; kegagalan ketahuan belakangan | Kegagalan/keterlambatan memicu peringatan otomatis; akses berbasis peran | Katalog, kualitas, dan *lineage* terintegrasi dan diaudit | Hasil audit dan pemantauan dipakai untuk menyempurnakan kemampuan platform secara berkelanjutan |
| **Kesiapan AI/BI** | AI dipakai bebas tanpa aturan | Ada minat dan usulan, belum ada kriteria kelayakan | Kriteria kelayakan AI dipakai untuk setiap usulan | *Use case* yang memenuhi kriteria kelayakan dievaluasi dampak dan risikonya | Hasil evaluasi *use case* dipakai untuk menyempurnakan kriteria kelayakan dan kebijakan AI secara berkala |

Uraian setiap tingkat bersifat usulan awal dan perlu disepakati dalam kalibrasi bersama unit terkait. Keterkaitan antara lima dimensi dan lima tingkat kapabilitas tersebut ditunjukkan pada Gambar 4.

![[Gambar 4 - Kerangka Penilaian Kematangan.excalidraw]]

*Gambar 4. Kerangka Penilaian Kematangan Tata Kelola Data yang Diusulkan, yang terdiri atas lima dimensi dan lima tingkat kapabilitas SPBE. Sumber: diolah dari DAMA-DMBOK dan PermenPANRB Nomor 59 Tahun 2020.*

**3. Cara menilai.** Penilaian dilakukan berbasis bukti yang dapat berupa dokumen kebijakan, SK penunjukan, SOP, atau log sistem. Bukti tersebut dilengkapi dengan wawancara atau lokakarya bersama DITTIK, pengelola data, dan unit keamanan informasi. Hasil sementara kemudian dibahas dalam kalibrasi bersama unit terkait sebelum ditetapkan. Suatu dimensi baru dinyatakan mencapai tingkat tertentu apabila seluruh syarat pada tingkat tersebut telah terbukti.

**4. Keluaran.** Keluaran penilaian berupa profil kematangan per dimensi yang digunakan untuk menetapkan prioritas *checkpoint*. Dimensi dengan tingkat terendah, misalnya, didahulukan penanganannya pada *Checkpoint* 1 dan 2.

## 8.6 *Checkpoint* Implementasi

*Checkpoint disusun sebagai urutan syarat yang harus diselesaikan sebelum pelaksanaan berlanjut ke tahap berikutnya, bukan sebagai jadwal bertanggal.* Kondisi yang harus tercapai pada setiap *checkpoint* disajikan pada tabel berikut, dan alurnya ditunjukkan pada Gambar 5.

| *Checkpoint* | Kondisi yang harus tercapai |
|---|---|
| **0: Arah siap** | • Pimpinan menetapkan domain pilot berdasarkan empat kriteria pemilihan.<br>• Penilaian kematangan dijalankan dengan kerangka lima dimensi dan menghasilkan profil kematangan per dimensi.<br>• Kebijakan/pedoman internal manajemen data dan klasifikasi disepakati.<br>• RACI ringkas untuk domain pilot ditetapkan.<br>• Daftar SOP yang perlu disusun disepakati sebagai *backlog* penulisan. |
| **1: Kendali dasar berjalan** | • Register objek/metrik prioritas mulai terisi.<br>• Kelas data terisi untuk objek prioritas.<br>• SOP gelombang 1 (klasifikasi, akses, register, rilis angka, masalah kualitas, kriteria kelayakan AI) sudah memiliki penanggung jawab penyusunan draf.<br>• *Lineage* tingkat 1 tersedia untuk beberapa metrik kritis. |
| **2: Angka laporan lebih dipercaya** | • Proses rilis angka dipakai untuk metrik prioritas.<br>• *Dashboard*/laporan prioritas dipetakan ke metrik resmi.<br>• Pemantauan dasar *pipeline* berjalan (kegagalan dan keterlambatan terlihat).<br>• Rapat berkala masalah data berjalan. |
| **3: AI terbatas boleh masuk** | • Kriteria kelayakan AI dipakai untuk setiap usulan baru.<br>• Ada 0–2 *use case* AI/BI terbatas yang memenuhi kriteria kelayakan. Jumlahnya dapat nol apabila belum ada yang siap; yang terpenting kriteria kelayakannya diterapkan.<br>• *Backlog* *use case* AI terpisah dari yang sudah disetujui. |
| **4: Perluasan terkendali** | • Domain tambahan masuk register.<br>• SOP gelombang 2 mulai ditulis sesuai kebutuhan.<br>• Pemetaan ke indikator Renstra/SPBE dilengkapi bila dokumen resminya sudah tersedia.<br>• Kebijakan dan RACI ditinjau ulang. |

![[Gambar 5 - Alur Checkpoint Implementasi.excalidraw]]

*Gambar 5. Alur Checkpoint Implementasi (Urutan Syarat, Bukan Jadwal Kalender). Sumber: hasil olahan kajian.*

Profil kematangan yang dihasilkan pada *Checkpoint* 0 digunakan untuk menetapkan prioritas *checkpoint* berikutnya. Penilaian ulang dengan kerangka yang sama dapat dilakukan untuk melihat kemajuan dari satu *checkpoint* ke *checkpoint* berikutnya.

**Kondisi yang harus dihentikan:** AI atau analitik di lingkungan *production* yang berjalan tanpa pemilik data dan kelas data, perubahan angka resmi tanpa catatan, serta pemberian akses data secara luas tanpa tinjauan.

## 8.7 Manajemen Risiko

Risiko utama dalam pelaksanaan tata kelola data beserta dampak dan cara penanganannya diidentifikasi sebagaimana tersaji pada tabel berikut.

| Risiko | Dampak | Cara menahan |
|----|----|----|
| R1 Fondasi dilompati (AI didahulukan, tata kelola belakangan) | Angka dan keputusan tidak andal; pelanggaran PDP | *Checkpoint* 0–2 wajib selesai sebelum *Checkpoint* 3 |
| R2 Definisi angka tidak seragam | Laporan saling bertentangan; kepercayaan hilang | Metrik resmi dan SOP perubahan definisi |
| R3 Akses terlalu luas atau data bocor | Risiko hukum dan reputasi | Klasifikasi, SOP akses, dan audit sederhana |
| R4 Ketergantungan pada mitra tanpa dokumentasi | Operasi macet saat personel berganti | Register dan alih pengetahuan dari mitra |
| R5 *Pipeline* tidak terpantau | Laporan kosong atau salah, terlambat diketahui | Pemantauan *job* dan eskalasi |
| R6 Data bayangan/salinan liar | Tidak jelas versi mana yang benar | Katalog dan jalur berbagi resmi |
| R7 Lokasi utama dan lokasi DR tanpa zona dan kebijakan yang sama | Data sensitif disimpan atau dipulihkan tanpa kendali yang setara | Zonasi dan kebijakan yang berlaku sama di semua lokasi |
| R8 Penolakan karena "SOP merepotkan" | Aturan tidak dipakai | Mulai dari domain pilot; SOP bertahap; bahasa sederhana |

Pemilik risiko pada prinsipnya adalah pejabat unit terkait untuk domainnya masing-masing, sedangkan masalah lintas unit diteruskan melalui jalur eskalasi ke pejabat berwenang.
