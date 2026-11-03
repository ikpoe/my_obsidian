# 2.4 Gambaran Kondisi Secara Umum

Subbab ini merangkum pola yang muncul di seluruh area pengelolaan data, yaitu di mana kebijakan dan praktik paling berjauhan, tema yang berulang lintas area, dan kekuatan yang dapat menjadi dasar pengembangan.

## Pola umum: kebijakan lebih maju daripada praktik

- **Kerangka kebijakan internal sudah kuat.** Perdir 4/2025 mengatur hampir semua area, mulai dari arsitektur dan pemodelan, kualitas data, master data, keamanan, sampai berbagi data (D1 hlm. 14–17, 39–58, 80–91).
- **Penerapannya belum mengikuti.** Banyak ketentuan belum punya prosedur, alat, atau pelaksana yang jelas. Pekerjaan data masih banyak dikerjakan manual dan berdasarkan permintaan (D2 hlm. 22, 24, 31).
- **Platform dan alat sudah tersedia, tetapi belum dipakai secara terkelola.** Platform data, alat integrasi, katalog metadata, dan alat BI sudah berjalan (D2 hlm. 20–22, 26). Yang belum ada adalah aturan pemakaiannya, misalnya standar model, target freshness, lineage, dan definisi metrik bersama.

## Kesenjangan paling lebar dan paling sempit

- **Paling lebar di area data induk.** Kebijakan mewajibkan satu repositori terpusat (D1 hlm. 88), sedangkan praktiknya masih manual tanpa SOP maupun tool (D2 hlm. 24).
- **Lebar juga pada retensi, keamanan di tingkat data, dan metadata.** Retensi belum diatur dan lineage belum ada (R1; D3). Klasifikasi dan kontrol akses sudah rinci di kebijakan, tetapi penerapannya pada elemen data belum terlihat [perlu konfirmasi].
- **Paling sempit pada pengamanan pertukaran eksternal.** Persyaratan PKS, enkripsi, dan pengamanan perimeter sudah berjalan (D1 hlm. 50–51; D2 hlm. 37; R1), walaupun polanya belum seragam antaraplikasi (D2 hlm. 40).
- **Relatif sempit pada pengambilan data ke platform dan analitik deskriptif.** CDC, ETL, dan dasbor sudah dipakai sehari-hari (D2 hlm. 20–22), meski kelengkapan dan kesegaran datanya belum diukur.

## Tema lintas area

- **Kepemilikan dan kewenangan atas data belum jelas.** Peran owner dan steward belum didefinisikan, dan kewenangan mengubah elemen data juga belum jelas (D2 hlm. 26). Akibatnya terlihat di banyak area: definisi berbeda, sumber kebenaran tunggal tidak jelas, dan keputusan diambil kasus per kasus (D2 hlm. 24; R1).
- **Ketergantungan pada titik tunggal.** SMILE menjadi hub integrasi (D2 hlm. 39), tim data menjadi jalur semua permintaan analitik (D2 hlm. 22), dan data lake serta DWH berbagi satu environment (D2 hlm. 21).
- **Jalur keluar data belum terstandar.** Konsumsi internal, akses langsung ke basis data, dan kolaborasi dengan mitra berjalan lewat jalur yang berbeda-beda (D2 hlm. 22, 39–40). Data contract, standar API, dan SLA belum ada.
- **Dokumentasi dan pengukuran tertinggal.** Data Building Block masih mengikuti ITMP 2019–2024 (D2 hlm. 31), pola model platform tidak terdokumentasi, dan kualitas data belum diukur rutin di pipeline (R1).
- **Kesiapan menghadapi UU PDP belum merata.** Kepatuhan PDP menjadi prioritas (R1), tetapi retensi, minimisasi, dan pengendalian akses di tingkat data belum berjalan [perlu konfirmasi].

## Kekuatan yang dapat dibangun

- **Dasar regulasi internal yang lengkap**, sehingga pengembangan berikutnya lebih banyak berupa penerapan daripada penyusunan kebijakan baru (D1).
- **Unit pengelola data sudah terbentuk** di bawah Deputi Manajemen Data dan Analitik (D2 hlm. 51).
- **Platform data dan alat utama sudah berjalan**, sehingga tidak perlu dibangun dari nol (D2 hlm. 20–22).
- **Inisiatif yang sedang berjalan bisa dilanjutkan**: uji konsep MDM kepesertaan (D3), rancangan Data Hub (D4), dan prosedur kualitas data (D2 hlm. 27; D6).
- **Kebutuhan bisnis cukup jelas**, misalnya forecasting, risiko likuiditas, dan pertukaran data dengan mitra (R1), sehingga prioritas pengembangan dapat diarahkan pada manfaat nyata.

## Catatan Review & Tindak Lanjut

*(Bagian ini dihapus pada versi final.)*

> **Catatan aset:** tidak ada gambar. Gambar tidak ditambahkan karena isi subbab berupa sintesis naratif, dan visual kesenjangan per area berisiko terbaca sebagai skor.

**Keputusan yang perlu diambil ikpu**
1. Judul 2.4 diganti menjadi "Gambaran Kondisi Secara Umum" (keputusan sebelumnya). Apakah tema lintas area di sini perlu diselaraskan dengan daftar tantangan di 2.5 agar tidak tumpang-tindih?
2. Apakah perlu ditambah satu gambar sederhana (misalnya peta tema lintas area), atau subbab ini cukup naratif?

**Konfirmasi ke klien atau verifikasi**
1. Penerapan klasifikasi dan kontrol akses di tingkat elemen data pada platform data.
2. Langkah kepatuhan UU PDP yang sudah berjalan, termasuk retensi dan minimisasi data.
