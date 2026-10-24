---
title: "BAB V SPESIFIKASI ARAH ARSITEKTUR DATA DAN INFORMASI"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB V SPESIFIKASI ARAH ARSITEKTUR DATA DAN INFORMASI

Bab ini menguraikan spesifikasi arah arsitektur data dan informasi. Arah arsitektur tersebut ditetapkan pada tingkat kemampuan yang harus tersedia beserta cara bagian-bagiannya saling terhubung, bukan sebagai resep konfigurasi layanan awan atau merek produk tertentu. Pemilihan teknologi menjadi materi implementasi lanjutan sepanjang memenuhi spesifikasi yang diuraikan, termasuk di lingkungan yang saat ini memakai layanan awan publik sebagai pusat data utama dan PUSDAKIM sebagai lokasi DR.

Arsitektur yang dituju mendukung terwujudnya rantai data yang terkelola mulai dari sumber, pengolahan, dan metrik hingga pemakaian untuk laporan dan AI. Beban operasional (OLTP) dipisahkan dari beban analitik atau *online analytical processing (OLAP).* Sementara itu, klasifikasi, akses, kualitas, dan *lineage* dikendalikan di sepanjang rantai tersebut.

## 5.1 Prinsip Penyusunan Arsitektur

Penyusunan arsitektur berpedoman pada tujuh prinsip berikut. Prinsip-prinsip tersebut menjaga agar pilihan teknologi pada tahap implementasi tetap berada dalam koridor tata kelola yang sama.

1.  **A1: Kemampuan sebelum merek.** Kebijakan menetapkan kemampuan yang wajib ada (katalog, kualitas, akses, dan *lineage*), sedangkan alat dapat diganti tanpa mengubah prinsip.

2.  **A2: OLTP terpisah dari analitik.** Sistem transaksi layanan tidak dijadikan penyimpanan laporan berat, sedangkan analitik menggunakan jalur, salinan, atau model data tersendiri.

3.  **A3: Kesepakatan data antarlapisan.** Setiap lapisan, yaitu sumber, pengolahan, metrik, dan pemakaian, terhubung melalui definisi, pemilik, dan kelas data yang disepakati.

4.  **A4: Keamanan dan PDP sejak desain.** Klasifikasi dan minimisasi melekat pada zona dan jalur data, termasuk ke kanal AI/BI.

5.  **A5: Pemantauan sebanding dengan data masuk.** Penjadwalan, kegagalan proses, anomali volume atau isi, dan jejak olahan dapat dipantau.

6.  **A6: Transisi bertahap.** Arsitektur dicapai bertahap per gelombang (domain atau metrik prioritas), bukan sekaligus.

7.  **A7: Sadar multi-lokasi.** Lokasi utama dan lokasi DR dikelola dengan zona kepercayaan, jalur data, dan kebijakan yang sama, tanpa mengunci topologi penyedia tertentu dalam kebijakan.

## 5.2 Arsitektur Target

Secara logis, arsitektur target terdiri atas enam lapisan sebagai berikut.

1.  **Sumber data operasional.** Lapisan ini mencakup aplikasi layanan dan pendukung SIMKIM, basis data transaksi, serta sumber eksternal, dengan stabilitas layanan sebagai prioritas dan perubahan struktur data yang dikendalikan.

2.  **Penyerapan dan integrasi.** Lapisan ini mencakup kemampuan mengambil atau menyalin data dari sumber, secara berkala atau mendekati waktu nyata sesuai dengan kebutuhan, yang dilengkapi katalog koneksi, kredensial terkendali, dan kesepakatan struktur data. Sinkronisasi dari banyak satuan kerja ke penyimpanan terpusat juga termasuk dalam lapisan ini sepanjang masih relevan.

3.  **Pengolahan dan penyimpanan analitik.** Lapisan ini mencakup transformasi data (ETL/ELT) yang tersusun, penyimpanan analitik yang terpisah dari OLTP, serta area data terkurasi untuk pelaporan. Orkestrasi *job* pada lapisan ini memiliki jadwal, ketergantungan, dan penanganan kegagalan yang jelas.

4.  **Semantik dan metrik.** Lapisan ini memuat definisi bisnis untuk indikator resmi, yang terdiri atas metrik tersertifikasi, kamus istilah minimum, dan riwayat perubahan definisi. Lapisan inilah yang menjembatani data teknis dengan angka yang dibaca pimpinan.

5.  **Pemakaian.** Lapisan ini mencakup laporan dan *dashboard*, layanan permintaan data, ekspor terkendali, serta fitur AI/BI di atas data yang memenuhi kriteria kelayakan apabila telah disetujui.

6.  **Bidang lintas lapisan.** Bidang ini mencakup katalog dan metadata, *lineage*, kualitas dan pemantauan, akses berbasis kelas data, pencatatan dan audit, serta pengelolaan konfigurasi *pipeline*.

**Alur logis:** data mengalir dari sumber operasional ke lapisan penyerapan dan integrasi, kemudian ke penyimpanan dan pengolahan analitik, lalu ke lapisan semantik dan metrik, hingga sampai pada pemakaian berupa laporan, permintaan data, atau AI yang memenuhi kriteria kelayakan. Katalog, *lineage*, kualitas, akses, dan audit melekat di sepanjang jalur tersebut.

## 5.3 Fondasi Semantik: Data Induk, Referensi, Standar, Metadata, dan Bagi Pakai

Sejalan dengan kebijakan Satu Data, fondasi semantik diawali dengan penetapan data induk dan data referensi untuk entitas yang dipakai lintas laporan, misalnya referensi wilayah dan satuan kerja, status layanan, dan jenis dokumen. Daftar finalnya ditetapkan per domain pada tahap implementasi. Fondasi tersebut dilengkapi standar data minimum yang mengatur tipe, format, nilai yang sah, serta aturan kelengkapan untuk objek prioritas. Metadata baku mencakup sistem asal, pemilik, *steward*, kelas klasifikasi, frekuensi pembaruan, dan definisi metrik. Bagi pakai data dilakukan melalui jalur resmi, seperti API, berkas, atau *pipeline*, yang mencatat tujuan, kelas data, dan akses sehingga salinan liar tanpa register dapat dihindari.

## 5.4 Klasifikasi, Zonasi, Keamanan Informasi, dan PDP

Klasifikasi informasi menentukan zona kepercayaan beserta kendalinya. Secara logis, jalur dan area data publik dan internal dipisahkan dari data terbatas dan rahasia, sedangkan data sensitif atau data pribadi tidak masuk ke kanal eksplorasi terbuka tanpa dasar pemrosesan, *masking*, atau minimisasi. Hak akses diberikan berdasarkan prinsip *least privilege*, dan hak teknis untuk mengurus *pipeline* tidak secara otomatis memberikan hak membaca angka resmi atau data mentah yang sensitif. Pengamanan teknis meliputi enkripsi, pengelolaan kredensial rahasia, dan catatan audit untuk akses data kelas tinggi, termasuk pada salinan di lokasi DR. Kanal AI/BI hanya memakai objek yang kelasnya mengizinkan, disertai pencatatan pertanyaan dan perintah kepada AI apabila relevan pada tahap implementasi serta pencatatan tujuan dan masa simpan data pelatihan maupun data masukan AI.

## 5.5 Kualitas Data, *Lineage*, dan Pemantauan

Kualitas data dijaga melalui aturan kualitas minimum untuk objek dan metrik prioritas yang mencakup kelengkapan, keabsahan, dan kewajaran antarperiode. Untuk indikator kritis, *lineage* tingkat 1 menghubungkan sistem sumber utama, olahan utama, metrik, dan pemakai. Pemantauan *pipeline* mencakup status *job*, keterlambatan, dan anomali volume, disertai peringatan kepada *steward* atau tim teknis. Sebelum angka resmi dipakai pimpinan, proses rilis angka menggunakan *checklist* yang terkait dengan sasaran S4.

## 5.6 Optimalisasi Infrastruktur dan Basis Data

Optimalisasi infrastruktur dan basis data diarahkan pada pemisahan beban transaksi dan beban analitik sehingga sistem transaksi layanan tidak dijadikan gudang laporan berat. Ukuran penyimpanan analitik dan jumlah koneksi sumber dikelola melalui register dan siklus hidup data, yaitu retensi, arsip, dan penghapusan. Apabila keputusan ukuran implementasi membutuhkan angka kapasitas, pengukuran ulang skala data perlu dilaksanakan terlebih dahulu. Inventori *instance* basis data dimanfaatkan untuk merasionalisasi dan memperjelas peran setiap basis data, baik sebagai OLTP, pendukung, maupun analitik, tetapi tidak menjadi satu-satunya acuan kapasitas *data warehouse*. Pilihan mesin basis data, *data warehouse*, atau *lakehouse* merupakan keputusan implementasi. Ketentuan ini hanya menetapkan pembagian tanggung jawab antarlapisan dan kendali tata kelola.

## 5.7 Strategi Transisi Arsitektur

Transisi menuju arsitektur target dilaksanakan dalam empat gelombang kendali sebagaimana disajikan pada tabel berikut.

| Gelombang | Isi |
|----|----|
| 0: Landasan kendali | Skema klasifikasi, RACI ringkas, register koneksi/objek prioritas, dan *lineage* awal untuk indikator kritis pada domain pilot. |
| 1: Rantai pelaporan prioritas | Metrik tersertifikasi, peta pemakaian *dashboard*/laporan prioritas, serta kualitas dan pemantauan minimum. |
| 2: Perluasan domain dan interoperabilitas | Data induk/referensi, bagi pakai terkendali, dan pengurangan salinan liar. |
| 3: AI melalui kriteria kelayakan | Kanal AI/BI hanya untuk objek yang memenuhi kriteria kelayakan. *Backlog* *use case* terpisah dari *production*. |

Setiap gelombang memiliki kriteria selesai yang selaras dengan *checkpoint* pelaksanaan. Migrasi antarplatform, termasuk rencana jangka panjang memindahkan aplikasi ke PUSDAKIM, juga mengikuti gelombang kendali tersebut. Hal tersebut bertujuan untuk mencegah terulangnya pola perpindahan infrastruktur yang tidak disertai peningkatan tata kelola.

## 5.8 Batasan Spesifikasi Arsitektur

Spesifikasi arsitektur tidak menetapkan nama layanan awan, paket produk, maupun topologi jaringan yang rinci. Desain tim platform AI dan perangkat MLOps yang lengkap berada di luar spesifikasi ini, dan kajian ini hanya mengatur batas tata kelolanya. Adapun daftar lengkap API dan struktur fisik data menjadi bagian dari tahap implementasi.
