---
title: "BAB VII PENGEMBANGAN KAPASITAS DAN BUDAYA DATA"
project: Imigrasi/AI Readiness
tags: [data-governance, ai-readiness, kajian]
source: "https://docs.google.com/document/d/1GYkispSmnatAuMVhNaGzjwpnfZAWdWu8pft0zwWU5qE/edit?tab=t.0"
---

# BAB VII PENGEMBANGAN KAPASITAS DAN BUDAYA DATA

Tata kelola data hanya dapat berjalan apabila didukung oleh SDM dan kebiasaan kerja yang sesuai. Pembahasan dalam bab ini difokuskan pada kompetensi untuk tata kelola data dan pelaporan, literasi data bagi pejabat yang memakai angka, alih pengetahuan dari mitra *managed service* kepada pegawai internal, serta budaya kerja yang menghargai definisi angka dan klasifikasi data. Pembentukan tim platform AI berada di luar cakupan bab ini.

## 7.1 Kompetensi yang Dibutuhkan

Pelaksanaan tata kelola data tidak harus disertai penambahan banyak jabatan baru. Hal yang lebih menentukan adalah tersedianya personel yang mampu menjalankan peran tata kelola data. Pemilik data sebagai pengambil keputusan atas angka perlu memahami definisi metrik dan berani mengesahkan atau menolak rilis angka. *Steward* data mengurus register, kualitas harian, kelas data, dan komunikasi dengan pemakai. Tim teknis data dan platform mengurus *pipeline*, akses teknis, dan pemantauan *job* data, sementara fungsi kepatuhan dan keamanan informasi menjaga batas PDP dan akses. Pemakai laporan dan AI pun perlu memahami batas kelas data dan tidak menganggap keluaran AI otomatis sebagai angka resmi.

Peta jabatan rinci dan formasi ASN mengikuti ketentuan unit kepegawaian dan SOTK. Pada tahap awal, peran dapat dirangkap melalui surat penunjukan sederhana sepanjang pembagian RACI-nya jelas.

## 7.2 Pengembangan Kapasitas

Pengembangan kapasitas diarahkan pada kegiatan praktis dan tidak berbentuk kurikulum yang panjang. Pemilik data dan *steward* memperoleh pembekalan singkat selama satu sampai dua hari mengenai klasifikasi, register, rilis angka, dan kriteria kelayakan AI. Pengelola *pipeline* memperoleh pelatihan teknis bertahap mengenai pemantauan, kualitas, dan metadata. Pemakai *dashboard* memperoleh sosialisasi tentang cara membaca angka beserta sumber dan tanggal potong datanya. Selain itu, unit terkait dapat menyusun satu atau dua SOP prioritas secara bersama-sama dengan pendekatan belajar sambil bekerja. Keberhasilan pengembangan kapasitas diukur secara sederhana, yaitu terisinya peran dan register serta tersedianya personel yang mampu menjelaskan asal-usul suatu angka.

## 7.3 Literasi Data

Literasi data yang dibutuhkan mencakup kemampuan dasar untuk membedakan angka resmi dari angka uji coba, membaca definisi singkat metrik, dan memahami bahwa kelas data menentukan data yang boleh dibagikan. Literasi tersebut juga mencakup kesadaran untuk tidak menyebarkan data sensitif melalui kanal informal. Sasaran utamanya adalah pejabat pemakai laporan dan staf yang sering meminta data. Materi literasi disajikan secara ringkas, misalnya dalam bentuk lembar satu halaman atau video singkat.

## 7.4 Alih Pengetahuan dan Pengurangan Ketergantungan pada Mitra

Saat ini, banyak pekerjaan data melibatkan mitra *managed service*. Kondisi tersebut wajar, tetapi menimbulkan risiko apabila hanya mitra yang memahami isi *pipeline* dan definisi angka. Untuk mengurangi risiko tersebut, dokumentasi minimum perlu tersimpan di register dan tidak hanya berada dalam ingatan personel mitra. Pegawai internal yang ditunjuk juga memerlukan akses bersama yang terkendali, dan serah terima dilakukan setiap kali kontrak atau personel berganti. Pada prinsipnya, mitra membantu pelaksanaan pekerjaan, sedangkan peran pemilik data, pengesahan angka resmi, serta keputusan tentang makna data dan kepatuhan tetap berada di pihak internal.

## 7.5 Budaya Kerja Berbasis Data

Budaya kerja berbasis data yang ingin ditumbuhkan menempatkan setiap angka penting memiliki pemilik, bukan sekadar bersumber dari *dashboard*. Perubahan definisi dibicarakan secara terbuka, klasifikasi tetap dipatuhi meskipun terasa merepotkan, dan koreksi angka dianggap wajar sepanjang dicatat. Usulan AI juga diuji terhadap kriteria kelayakan dan tidak langsung masuk ke *production*. Budaya tersebut dapat didorong melalui pencantuman pemilik metrik pada laporan penting, rapat singkat bulanan tentang masalah data, dan pemanfaatan domain pilot yang telah tertata sebagai contoh bagi unit lain.

## 7.6 Batasan Pembahasan

Struktur tim platform AI tidak diuraikan dalam bab ini. Formasi jabatan fungsional secara rinci dan modul pelatihan yang lengkap juga berada di luar cakupan pembahasan dan dapat disusun pada tahap implementasi.
