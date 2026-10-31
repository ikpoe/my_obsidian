---
type: klarifikasi
project: BPJSTK-Interoperabilitas-Data
area: Tata Kelola Data
sesi: 7
status: siap-dikirim
tags: [klarifikasi, tata-kelola-data, pdp, berbagi-data]
---
# Daftar Klarifikasi — Tata Kelola Data

> Konteks: daftar ini untuk **tim data governance BPJS** yang hadir pada pertemuan minggu depan (R1 [P 02:09:11–02:09:31]). Penilaian awal berbasis Perdir 4/2025, paparan EA As-Is, slide status kapabilitas, Q&A 31 Okt 2026, dokumen teknis API Kemnaker, dan rekaman diskusi 27 Okt 2026. Pertanyaan diurutkan menurut prioritas: **P1** = menentukan rekomendasi model operasi; **P2** = menentukan desain; **P3** = melengkapi skor. Rujukan: [[17-Tata-Kelola-Data]].

## A. Organisasi & Mandat (P1)
1. Apa mandat tertulis Asdep Data Governance dan bagaimana pembagian tugasnya dengan Asdep Interoperability & Processing dan Data Collaboration? Apakah struktur di paparan EA (berdasar Perdir organisasi 2024) masih berlaku?
2. Perdir 2015 tentang Organisasi dan Pedoman Data Governance Kepesertaan dicabut oleh Perdir 4/2025. Apa yang menggantikan pengaturan peran di dalamnya (SK, SOP, atau belum ada)?
3. Berapa orang di tim data governance saat ini, dan apa program kerja 2026–2027-nya?
4. Apakah KPTI pernah membahas atau memutus isu data (definisi, kualitas, berbagi data)? Bersediakah tim membagikan risalah dua rapat terakhir?

## B. Peran & Hak Keputusan (P1)
5. Apakah data owner dan data steward sudah ditunjuk untuk domain mana pun? Jika ya, di mana tercatat?
6. Siapa yang saat ini berwenang menyetujui perubahan elemen data, definisi glossary lintas bidang, dan aturan penggabungan master data (mis. kasus merger perusahaan)?
7. Apakah tim lebih condong ke kantor tata kelola terpusat, federasi (hub + owner/steward di unit bisnis), atau cukup diselipkan ke tugas unit yang ada? Apa kendala organisasinya?
8. Apakah Unit Bisnis bersedia menunjuk pejabat sebagai data owner yang akuntabel atas akses, berbagi, dan retensi domainnya?

## C. Kebijakan & Standar (P2)
9. Juknis atau SOP turunan Perdir 4/2025 (Ps. 3(2)) apa saja yang sudah terbit atau sedang disusun? Mohon daftarnya.
10. Apakah ada SOP pengelolaan master data dan SOP pertukaran data? Rekaman menyebut keduanya belum ada; apakah sedang disusun?
11. Apakah Perdir PDP (Ps. 3(4)) sudah terbit, dan apakah pejabat pelindungan data pribadi sudah ditunjuk? Bagaimana hubungannya dengan UMD dan OKI?

## D. Berbagi Data Eksternal (P1)
12. Bagaimana alur PKS berjalan saat ini dari usulan sampai operasional? Apakah UMD selalu dilibatkan (Ps. 73a)?
13. Apakah ada register PKS terpusat (mitra, tujuan, elemen data, kanal, tanggal berakhir, status adendum Ps. 78)?
14. Siapa yang menyusun dan menyetujui dokumen teknis yang menjadi dasar eligibility Data Hub? Apakah ada templat baku?
15. Untuk Kemnaker: siapa pemilik hubungan dan PKS-nya? Apakah PKS mengatur retensi dan pemusnahan salinan data di sisi Kemnaker, minimisasi dua arah, dan hak audit? Apakah ada rencana memindahkan akses DB link ke Data Hub/3scale?
16. Bagaimana mencegah unit bisnis meminta data yang sama ke mitra berulang kali? Apakah tim bersedia menjadi satu pintu permintaan?

## E. Retensi & Minimisasi (P2)
17. Siapa yang akan memiliki kebijakan retensi (data owner, UMD, Hukum, atau pejabat PDP)? Apakah ada persiapan menghadapi PP 33/2026 (berlaku 16 Jan 2027)?
18. Apakah ada aturan minimisasi elemen saat menerima atau membagikan data pribadi?

## F. Isu, Metrik & Tooling (P3)
19. Bagaimana isu data (bukan insiden TI) dicatat dan dieskalasi saat ini? Tool apa yang dipakai?
20. Apakah ada KPI tata kelola data dalam kontrak kinerja unit (mis. metrik kualitas sebagai indikator unit, D1 hlm. 81)?
21. Apakah Dataedo direncanakan menyimpan owner, steward, klasifikasi, dan tautan PKS? Siapa admin bisnisnya?
22. Apakah 3scale sudah dipakai untuk memetakan satu PKS ke satu application plan dengan kuota dan log?
