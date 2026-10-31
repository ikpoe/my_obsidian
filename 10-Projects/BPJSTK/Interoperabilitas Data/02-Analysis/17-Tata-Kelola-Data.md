---
type: analysis
project: BPJSTK-Interoperabilitas-Data
area: Tata Kelola Data
sesi: 7
blueprint: [3.1, 3.2, 3.3, 3.4, 4, 5.3, 5.4, 6]
status: draft-diskusi
sumber: [D1 Perdir 4/PERDIR.06/042025, D2 EA As-Is, D3 status kapabilitas & pola MDM, D4 Q&A WA 31 Okt 2026 (kebijakan Data Hub), D5 dokumen teknis API Kemnaker (RESTRICTED, hanya temuan), R1 rekaman diskusi 27 Okt 2026, UU 27/2022, PP 33/2026, 00-Analisis-Dokumen-BPJS.md, 11-Master-Data.md, 12-Integrasi.md, 13-Arsitektur-Data.md, 14-Pemodelan-Data.md, 15-Metadata-Kualitas.md, 16-Keamanan-Data.md]
konvensi_halaman: "hlm. = nomor halaman PDF (penanda =====Pn===== di d1p/d2p). Di D1 mulai Lampiran, nomor cetak = hlm. PDF − 1 (mis. hlm. 87 = cetak '- 86 -')."
---

# Sesi 7 — Tata Kelola Data

> **Sumber rujukan.** **D1** = Peraturan Direksi BPJS Ketenagakerjaan No. 4/2025 tentang Pedoman Tata Kelola Data dan TI (kebijakan, kondisi yang seharusnya). **D2** = dokumen Enterprise Architecture BPJS Ketenagakerjaan (paparan kondisi saat ini/as-is). **D3** = tujuh slide BPJS (D3-01 status RAG kapabilitas; D3-03 pola MDM). **D4** = Q&A WhatsApp 31 Okt 2026. **D5** = dokumen teknis API Kemnaker (RESTRICTED; hanya temuan yang dikutip, tanpa nilai kredensial, alamat, atau nama host). **R1** = ekstraksi rekaman diskusi 27 Okt 2026, dirujuk [P hh:mm] / [S hh:mm]. "hlm." merujuk pada nomor halaman PDF; "Ps. N(M)" = Pasal N ayat (M).

> **Kerangka sesi.** Konsisten dengan sesi 1–6: ESDP adalah platform poros, dibangun dengan Opsi B di atas Cloudera CDP on-prem eksisting (zona staging | lake | DWH | serving, dipetakan ke landing/bronze/silver/gold). Integrasi adalah komponen ESDP: **upstream** = sumber → ESDP; **downstream** = ESDP → pengguna/mitra. SMILE adalah core system dengan lingkupnya sendiri (SoR), bukan bagian ESDP. Data Hub dibangun baru, ditempatkan di DMZ, khusus B2B, dengan kanal API (REST/JSON) atau SFTP melalui Red Hat 3scale (D4). Katalog federasi Atlas + Dataedo ([[15-Metadata-Kualitas]]); kontrol berbasis klasifikasi ([[16-Keamanan-Data]]); golden record MDM ([[11-Master-Data]]). Blueprint menetapkan **peran, keputusan, kebijakan, dan parameter yang wajib didefinisikan**, bukan nilainya.

## 1. Ringkasan
- **Struktur tata kelola formal ada, tetapi berorientasi TI, bukan data.** D1 membentuk struktur Direktur data & TI → KPTI → UPTI → Unit Bisnis → Kanwil → Kancab (Ps. 4(3), hlm. 7) dan KPTI yang rapat tiap semester (Ps. 6–7, hlm. 7–9). Tugas KPTI hampir seluruhnya tentang ITMP, anggaran, dan inisiatif TI (Ps. 6(1), hlm. 7–8). Belum ada forum keputusan khusus data (definisi, kepemilikan, sengketa kualitas, persetujuan berbagi).
- **Peran data owner, data steward, dan data custodian tidak didefinisikan di D1.** D1 hanya memuat prinsip "penetapan kepemilikan" untuk master data (Lampiran II A.1.b.2, hlm. 87), istilah *data liaison* (hlm. 89), dan kantor cabang sebagai *data producer* (Ps. 8(6), hlm. 10). D1 mencabut Perdir 2015 tentang *Organisasi dan Pedoman Data Governance Kepesertaan* (Ps. 79a, hlm. 62) tanpa menggantinya dengan model peran yang setara [perlu konfirmasi apakah diatur di SK/SOP lain].
- **Organisasinya ada di atas kertas.** D2 memetakan Asdep Data Governance, Interoperability & Processing, Data Analytics, dan Data Collaboration di bawah Deputi Manajemen Data & Analitik (D2 hlm. 51). D2 sendiri mencatat kewenangan perubahan elemen data tidak jelas (D2 hlm. 26) dan fungsi pengelola master data belum didefinisikan (D2 hlm. 24). R1 menyebut pembahasan data governance "tidak pernah masuk" dan tim pengelola data relatif baru (R1 [P 02:04:27–02:05:01; P 01:16:53–01:17:05]).
- **Berbagi data eksternal: kebijakannya paling lengkap, praktiknya paling berisiko.** D1 Bab VI mengatur skema, kriteria mitra, isi minimum PKS, valuasi data, dan alur usulan PKS (Ps. 68–73, hlm. 53–58). Praktiknya: belum ada SOP pertukaran dan PKS dapat diajukan siapa pun (R1 [S 00:13:48–00:15:07]); unit bisnis meminta data ke mitra tanpa satu pintu (R1 [S 00:13:22–00:16:35]); kasus Kemnaker menunjukkan akses DB link, salinan lokal tanpa retensi, masking satu arah, dan kredensial teks biasa (D5).
- **Retensi dan minimisasi tidak punya pemilik kebijakan.** D1 tidak memuat klausul retensi maupun minimisasi; D3-01 menandai Retention Policy merah; R1 mengonfirmasi kebijakan retensi belum ada (R1 [P 01:36:45–01:38:45]). PP 33/2026 (berlaku 16 Jan 2027) mewajibkan dokumen kebijakan masa retensi (Ps. 75) [perlu konfirmasi].
- **Skor usulan: Tata Kelola Data ±1,4** (14 aspek, rentang 1–2).
- **Rekomendasi (usulan, berbasis perbandingan):** **Opsi B — model federasi bertahap (hub-and-spoke).** Kantor tata kelola data (hub) di Unit Manajemen Data/Asdep Data Governance menetapkan kebijakan, standar, katalog, dan metrik. Data owner (pejabat Unit Bisnis) dan data steward per domain DBB menjadi spoke. Forum keputusan data dibentuk sebagai sub-forum KPTI. Tahun 1 berjalan lebih terpusat (hub kuat, 2–3 domain pilot), lalu didelegasikan ke domain di Tahun 2–3.

---

## 2. Kondisi saat ini (as-is)

| Komponen | Kondisi | Bukti | Catatan |
|---|---|---|---|
| Lapisan tata kelola | Digambar sebagai lapisan horizontal "Data Governance" | D2 hlm. 20 | Tanpa uraian kapabilitas atau proses |
| Organisasi data | Deputi Manajemen Data & Analitik; Asdep Data Governance, Interoperability & Processing, Data Analytics, Data Collaboration; dipetakan ke COBIT APO/MEA | D2 hlm. 51 | Dasar: Perdir organisasi 2024, lebih tua dari D1 [perlu konfirmasi struktur terkini] |
| Tim tata kelola data | Tim pengelola data/Dataedo berada di unit yang baru dibentuk; topik governance "tidak pernah masuk" | R1 [P 01:16:53–01:17:05; P 02:04:27–02:05:01] | Tim data governance diminta hadir minggu depan (R1 [P 02:09:11–02:09:31]) |
| Komite | KPTI ada, fokus TI | D1 Ps. 6–7 (hlm. 7–9) | Bukti rapat KPTI yang membahas isu data tidak ada [perlu konfirmasi] |
| Data owner / steward | Tidak ada daftar; Data Steward hanya di ilustrasi target MDM | D2 hlm. 23; D3-03 (lapisan Data Stewardship) | Ownership per domain belum diketahui, padahal diperlukan untuk consent dan pengungkapan (R1 [P 00:08:33–00:08:42; P 00:44:22–00:44:29]) |
| Hak keputusan | Kewenangan perubahan elemen data tidak jelas | D2 hlm. 26 | Risiko perubahan tanpa akuntabilitas (diakui D2) |
| SOP | Pengelolaan master data tidak diatur SOP | D2 hlm. 24 | SOP tata kelola pertukaran data juga belum ada (R1 [S 00:13:48–00:15:07]) |
| Keputusan master data | Merger perusahaan diputuskan kasus per kasus lewat rapat terbatas | R1 [P 01:06:06–01:08:42] | Keputusan steward tanpa aturan tertulis |
| PKS & berbagi eksternal | PKS dapat diajukan siapa pun; setelah sepakat baru masuk teknis (pembatasan akses, kuota) | R1 [S 00:13:48–00:15:07] | D1 Ps. 73a mensyaratkan koordinasi Unit Pemrakarsa dengan UMD (hlm. 57) |
| Eligibility Data Hub | PKS dan dokumen teknis yang disepakati antarinstansi; API atau SFTP; 3scale | D4 | Desain baik; penegakannya belum terlihat (D3-01 API Management merah) |
| Kasus Kemnaker | Akses DB link langsung; salinan lokal data di sisi Kemnaker; masking satu arah; kredensial teks biasa di file `.env` | D5 | Lihat §7; nilai sengaja tidak disalin |
| Retensi | Kebijakan tidak ada; data peserta bisa 40+ tahun | D3-01 merah; R1 [P 01:36:45–01:38:45; P 00:39:35–00:39:49] | Pemilik kebijakan belum ditunjuk |
| Tooling tata kelola | Dataedo pasif; Data Catalog, Data Lineage, Audit Trail merah | R1 [P 00:21:08–00:22:16]; D3-01 | Kebijakan tidak dapat ditegakkan lewat tool |
| Pelindungan data pribadi | Prioritas saat ini adalah kepatuhan UU PDP | R1 [P 00:08:50–00:09:01] | Perdir PDP (D1 Ps. 3(4)) belum diterima |

## 3. Kondisi yang seharusnya (D1)
- **Ps. 2(2)c, Ps. 3(1):** tata kelola data dan TI memenuhi prinsip kepatuhan, kemanfaatan, keamanan, integritas, ketersediaan, dan manajemen risiko; ruang lingkup mencakup struktur tata kelola, PDP, Kolaborasi dan Pemanfaatan Data, serta monev (D1 hlm. 5–6).
- **Ps. 3(2)–(4):** pengaturan teknis dapat dituangkan dalam peraturan atau surat petunjuk teknis Direktur TI dan/atau Kepala UPTI; teknis pelaksanaan mengacu standar lazim (antara lain COBIT); PDP diatur Perdir tersendiri (D1 hlm. 6). Ini dasar hierarki **Perdir → kebijakan/juknis → standar → SOP**.
- **Ps. 4–5:** struktur tata kelola (Direktur data & TI, KPTI, UPTI, Unit Bisnis, Kanwil, Kancab) dan manajemen risiko data & TI oleh UPTI bersama fungsi manajemen risiko (D1 hlm. 6–7).
- **Ps. 6–7:** KPTI diketuai Direktur data & TI, wakil Direktur manajemen risiko, anggota termasuk Kepala UMD; Kepala Unit Bisnis sebagai narasumber; rapat tiap semester, kuorum 2/3, hasil dapat dieskalasi ke rapat Direksi (D1 hlm. 7–9).
- **Ps. 8(3)–(6):** UMD **mengusulkan kebijakan** dan mengoordinasikan rencana tata kelola data (kualitas, pemutakhiran/koreksi, master & referensi, DWBI/big data); Unit Bisnis menyampaikan kebutuhan; Kanwil meningkatkan awareness dan kualitas data; Kancab sebagai *data producer* (D1 hlm. 9–10).
- **Ps. 10(2)h–i:** perencanaan tata kelola data dan pengembangan kompetensi organisasi data masuk ITMP (D1 hlm. 11–12).
- **Ps. 15, 31, 32:** UMD mengelola metadata, Katalog Data (keamanan berdasarkan klasifikasi), serta master & referensi data, dengan evaluasi tahunan (D1 hlm. 17, 29–30).
- **Lampiran II A:** prinsip MD&DR: shared, **penetapan kepemilikan**, kualitas, perubahan terkendali; repository tunggal; *data liaison*; hak akses terbatas; reviu tahunan (D1 hlm. 87–89).
- **Ps. 34–37:** pengelolaan permintaan, insiden, dan masalah layanan data & TI, termasuk eskalasi dan prioritas dampak (D1 hlm. 31–32).
- **Ps. 66, 68–73:** keamanan interoperabilitas; skema verifikasi, DaaS, pertukaran; kriteria mitra Non-KL; isi minimum PKS (tujuan, metode integrasi, elemen data, keamanan & kerahasiaan, narahubung); valuasi data dievaluasi minimal 2 tahunan; usulan PKS oleh Unit Pemrakarsa berkoordinasi dengan UMD; NDA untuk data sampel; monitoring oleh UMD dan evaluasi mitra oleh Unit Pemrakarsa (D1 hlm. 50–51, 53–58).
- **Ps. 74–77:** monev kinerja, pengendalian internal (termasuk *control exception* dan pihak ketiga), dan kepatuhan regulasi eksternal minimal 2 tahunan (D1 hlm. 58–61).
- **Ps. 78:** PKS eksisting wajib diadendum sesuai Ps. 68–73 paling lambat 6 bulan setelah Perdir ditetapkan (D1 hlm. 61–62).
- **Yang tidak diatur D1:** definisi peran data owner/steward/custodian, forum keputusan khusus data, RACI keputusan data, **retensi**, dan **minimisasi** data.

**Catatan regulasi eksternal (ringkas).** UU 27/2022 mensyaratkan pemrosesan terbatas sesuai tujuan (Ps. 16(2)), kewajiban pengendali atas dasar dan tujuan pemrosesan (Ps. 27–28), pengakhiran pemrosesan dan pemusnahan/penghapusan (Ps. 42–45), serta penunjukan **pejabat atau petugas pelindungan data pribadi** (Ps. 53; terverifikasi dari teks UU) untuk pemrosesan layanan publik dan data skala besar. PP 33/2026 (berlaku 16 Jan 2027): minimisasi data pribadi (Ps. 59) dan dokumen kebijakan masa retensi oleh pengendali (Ps. 75) [perlu konfirmasi teks final].

## 4. Tabel penilaian — Tata Kelola Data

Aturan skor: 1 = Awal; 2 = Berulang (atau batas atas jika hanya ada kebijakan); 3 = Terdefinisi; 4 = Terkelola; 5 = Optimal.

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2/D3/R1/D5) | Gap | Skor usulan (1-5) |
|---|---|---|---|---|---|
| TK-1. Struktur & forum keputusan (komite) | Forum keputusan data dengan mandat, agenda, dan risalah; eskalasi ke KPTI/Direksi | KPTI Ps. 6–7 (hlm. 7–9), fokus TI | Bukti KPTI memutus isu data tidak ada; governance "tidak pernah masuk" (R1 [P 02:04:27–02:05:01]) | Forum khusus data | 2 (batas kebijakan) |
| TK-2. Peran data owner, steward, custodian | Peran terdefinisi, ditunjuk per domain, tercatat di katalog | Hanya prinsip kepemilikan MD&DR (hlm. 87), *data liaison* (hlm. 89), *data producer* (Ps. 8(6), hlm. 10) | Data Steward hanya ilustrasi (D2 hlm. 23; D3-03); owner per domain tidak diketahui (R1 [P 00:44:22–00:44:29]) | Definisi & penunjukan | 1 |
| TK-3. Kantor tata kelola data (hub) | Unit dengan mandat, kapasitas, dan program kerja | UMD Ps. 8(3) (hlm. 9) | Asdep Data Governance ada (D2 hlm. 51); tim relatif baru (R1 [P 01:16:53–01:17:05]) | Mandat & program kerja terlihat | 2 [perlu konfirmasi] |
| TK-4. Kerangka kebijakan & standar (hierarki) | Perdir → kebijakan/juknis → standar → SOP, ter-versi dan dipublikasi | Ps. 3(2)–(3) (hlm. 6); rujukan SOP UMD (hlm. 85–86) | SOP master data tidak ada (D2 hlm. 24); SOP pertukaran tidak ada (R1 [S 00:13:48–00:15:07]) | Lapis standar & SOP | 2 (batas kebijakan) |
| TK-5. Hak keputusan / RACI | RACI per jenis keputusan data (definisi, akses, kualitas, berbagi, retensi) | Tidak eksplisit; peran tersebar Ps. 8, 73 | Kewenangan perubahan elemen data tidak jelas (D2 hlm. 26); keputusan merger kasus per kasus (R1 [P 01:06:06–01:08:42]) | RACI belum ada | 1 |
| TK-6. Tata kelola berbagi data eksternal | Satu pintu; eligibility dari PKS + dokumen teknis; elemen minimum; persetujuan tercatat | Ps. 66, 68–71 (hlm. 50–56) | Kebijakan Data Hub mensyaratkan PKS + dokumen teknis via 3scale (D4); PKS bisa diajukan siapa pun dan tanpa SOP (R1 [S 00:13:48–00:15:07]); permintaan tanpa satu pintu (R1 [S 00:13:22–00:16:35]); API Management merah (D3-01) | Penegakan & satu pintu | 2 |
| TK-7. Siklus hidup perjanjian berbagi & kepatuhan mitra | Register PKS; reviu berkala; audit pemakaian; pengakhiran & pemusnahan salinan | Ps. 71(4), 72(4), 73e–f, 78 (hlm. 56–58, 61–62) | Kemnaker: DB link, salinan lokal tanpa retensi, masking satu arah, kredensial teks biasa (D5); status adendum Ps. 78 tidak diketahui | Register & monitoring | 1 |
| TK-8. Stewardship MDM & CDE | Steward per entitas master/CDE, antrean keputusan dengan SLA | Lampiran II A (hlm. 87–89); CDE di DQ (hlm. 85) | Lapisan Data Stewardship hanya di desain (D3-03); daftar CDE tidak ada ([[15-Metadata-Kualitas]] KD-2) | Steward operasional | 1 |
| TK-9. Manajemen isu & eskalasi data | Register isu data, triase steward, eskalasi ke forum | Ps. 34–37 (insiden TI, hlm. 31–32); alur DQ (hlm. 82–83); eskalasi Ps. 7(1)e | Proses insiden TI ada; perbaikan bad data di cabang belum optimal (D2 hlm. 28) | Jalur isu data terpisah dari insiden TI | 2 [perlu konfirmasi] |
| TK-10. Metrik & monev tata kelola | KPI tata kelola dilaporkan ke forum secara berkala | Ps. 74–77 (hlm. 58–61), fokus kinerja TI; metrik DQ dapat jadi indikator unit (hlm. 81) | Dashboard Kualitas Data (D2 hlm. 22); KPI tata kelola tidak terlihat | KPI governance | 1 |
| TK-11. PDP & pejabat pelindungan data pribadi | Pejabat PDP ditunjuk; dasar pemrosesan per dataset; tautan ke data owner | Ps. 3(4) dirujuk ke Perdir tersendiri (hlm. 6) | Prioritas kepatuhan UU PDP (R1 [P 00:08:50–00:09:01]); Perdir PDP & penunjukan pejabat belum terbukti | Peran & dokumen | 1 [perlu konfirmasi] |
| TK-12. Kepemilikan kebijakan retensi & minimisasi | Pemilik kebijakan, jadwal retensi per kelas data, termasuk salinan mitra | Tidak ada di D1 | Retention Policy merah (D3-01); belum ada (R1 [P 01:36:45–01:38:45]); salinan Kemnaker tanpa retensi (D5) | Kebijakan & pemilik | 1 |
| TK-13. Penautan tooling (katalog, lineage, 3scale) | Owner/steward, klasifikasi, kontrak, dan PKS tercatat di katalog; 3scale menegakkan eligibility | Katalog Data Ps. 31 (hlm. 29) | Dataedo pasif (R1 [P 00:21:08–00:22:16]); Catalog, Lineage, Audit Trail, API Management merah (D3-01) | Tool belum menegakkan kebijakan | 1 |
| TK-14. Budaya, awareness & kompetensi | Program literasi data; kompetensi steward | Ps. 8(5)a (hlm. 10); Ps. 10(2)i (hlm. 12) | Awareness entry data belum menyeluruh (D2 hlm. 28) | Program terstruktur | 1 |

**Rata-rata Tata Kelola Data: ±1,4** (19/14).

## 5. Opsi model operasi tata kelola data

| Opsi | Deskripsi |
|---|---|
| A. Kantor tata kelola terpusat | Satu kantor (di UMD/Asdep Data Governance) memegang kebijakan, definisi, persetujuan akses dan berbagi, serta stewardship semua domain |
| B. Federasi hub-and-spoke (bertahap) | Hub (kantor tata kelola data di UMD, dipimpin pejabat setingkat CDO) menetapkan kebijakan, standar, katalog, metrik; data owner (pejabat Unit Bisnis) dan steward per domain DBB memutus hal domain; forum keputusan data sebagai sub-forum KPTI |
| C. Tertanam di unit eksisting | Tidak ada kantor baru; fungsi dibagi ke KPTI, UMD, OKI, Unit Pemrakarsa, Hukum, dan Unit Bisnis sesuai tugas yang sudah ada di D1 |

**Perbandingan (++ sangat baik, + baik, 0 netral, − kurang, −− buruk)**

| Kriteria | A | B | C |
|---|---|---|---|
| Kesesuaian dengan D1 (UMD mengusulkan kebijakan, Unit Bisnis pemilik kebutuhan; Ps. 8) | + | ++ | + |
| Mengisi gap owner/steward dan RACI (D2 hlm. 24, 26) | + | ++ | −− |
| Skalabilitas untuk 5 shared dimension + domain DBB (D2 hlm. 15) | −− (hub jadi bottleneck) | ++ | − |
| Kecepatan keputusan berbagi eksternal (PKS, Data Hub) | 0 | + | −− (tersebar, tanpa pemilik) |
| Kebutuhan SDM dan perubahan organisasi | − | 0 | ++ |
| Kesiapan saat ini (tim baru, governance "belum pernah masuk") | + | 0 (perlu bertahap) | + |
| Akuntabilitas PDP (pejabat PDP ↔ data owner) | + | ++ | − |
| Keberlanjutan setelah proyek | 0 | + | −− |

**Rekomendasi (usulan): Opsi B, dijalankan bertahap.** Bukti menunjukkan masalah intinya adalah **akuntabilitas**: kewenangan elemen data tidak jelas (D2 hlm. 26), fungsi pengelola master data tidak ada (D2 hlm. 24), dan PKS bisa diajukan siapa pun (R1). Opsi C mempertahankan pola itu. Opsi A cocok untuk memulai, tetapi akan menjadi bottleneck saat Data Hub dan MDM melayani banyak domain dan mitra. Opsi B memakai struktur D1 apa adanya (UMD = hub, Unit Bisnis = owner, Kancab = producer, KPTI = eskalasi) dan hanya menambah dua hal: definisi peran owner/steward/custodian dan forum keputusan data. Fase: **Tahun 1** hub kuat (mendekati A) dengan 2–3 domain pilot (Kepesertaan, Pemberi Kerja, berbagi eksternal); **Tahun 2–3** delegasi ke steward domain. Status struktur Deputi/Asdep terkini [perlu konfirmasi] menentukan posisi hub.

## 6. Model peran (usulan Blueprint)

| Peran | Diisi oleh (usulan) | Tanggung jawab inti | Dasar |
|---|---|---|---|
| Direksi / KPTI | Direktur data & TI (ketua) | Eskalasi akhir, pengesahan kebijakan data, prioritas investasi | Ps. 6–7 |
| Forum Tata Kelola Data (sub-forum KPTI) | Kepala UMD (ketua), data owner domain, OKI, pejabat PDP, Hukum, Manajemen Risiko | Memutus definisi lintas domain, sengketa kualitas, persetujuan berbagi berisiko tinggi, kebijakan retensi | Usulan; Ps. 6(1)d, 7(1)e |
| Kantor tata kelola data (hub, setingkat CDO) | UMD / Asdep Data Governance | Kebijakan & standar, katalog, metrik, register PKS, koordinasi steward | Ps. 8(3), 15, 31–32, 73 |
| Data owner | Pejabat Unit Bisnis per domain DBB | Akuntabel atas definisi, klasifikasi, akses, berbagi, retensi domainnya | Lampiran II A.1.b.2 (hlm. 87) — perlu diperluas ke semua domain |
| Data steward (bisnis) | Analis senior di Unit Bisnis/Kanwil | Definisi, CDE, triase isu DQ, keputusan antrean MDM | Usulan; D3-03 |
| Data steward (teknis) / *data liaison* | UMD per domain | Kamus Data, kontrak data, lineage, publikasi master data | hlm. 89 |
| Data custodian | Unit Operasional TI; tim platform ESDP; pengelola 3scale | Operasional penyimpanan, akses, backup, eksekusi retensi & pemusnahan | Ps. 8(2) |
| Data producer | Kantor cabang, pemilik aplikasi SoR (SMILE) | Mutu input, perbaikan di sumber | Ps. 8(6) |
| Pejabat pelindungan data pribadi | Ditunjuk Direksi [perlu konfirmasi] | Kepatuhan UU PDP, dasar pemrosesan, DPIA, insiden | UU 27/2022 Ps. 53 |
| OKI (CISO, ISM) | – | Kebijakan keamanan, persetujuan akses eksternal | Ps. 50, 66 |
| Unit Pemrakarsa | Unit yang menginisiasi PKS | Usulan PKS, evaluasi kinerja mitra | Ps. 73a, f |

## 7. Kerangka kebijakan, standar, dan prosedur

| Tingkat | Instrumen | Isi wajib (tanpa nilai) | Pemilik |
|---|---|---|---|
| 1. Perdir | Perdir 4/2025; Perdir PDP (Ps. 3(4)) | Prinsip, struktur, kewenangan | Direksi |
| 2. Kebijakan / juknis (Ps. 3(2)) | Kebijakan peran & hak keputusan data; kebijakan berbagi data; kebijakan retensi & minimisasi | Peran, RACI, forum, eskalasi; dasar eligibility; kelas retensi | Direktur TI / Kepala UPTI, usulan UMD |
| 3. Standar | Klasifikasi & label PDP; glossary/Kamus Data; kualitas (dimensi, CDE); kontrak data; dokumen teknis pertukaran; metadata wajib katalog | Atribut, format, ambang yang harus ditetapkan | UMD (hub) dengan owner domain |
| 4. Prosedur / SOP | Penunjukan owner/steward; perubahan elemen data; permintaan akses; siklus PKS; manajemen isu data; retensi & pemusnahan | Langkah, peran, SLA (parameter) | UMD, Unit Pemrakarsa, Unit Operasional TI |

## 8. Hak keputusan (RACI ringkas, usulan)

R = Responsible, A = Accountable, C = Consulted, I = Informed.

| Keputusan | Forum TKD | Hub (UMD) | Data owner | Steward | Custodian (Ops TI/ESDP) | OKI | Pejabat PDP | Unit Pemrakarsa |
|---|---|---|---|---|---|---|---|---|
| Definisi istilah/elemen dalam domain | I | C | A | R | I | – | – | – |
| Definisi lintas domain / shared dimension | A | R | C | C | I | – | – | – |
| Klasifikasi & label PDP | I | R | A | C | I | C | C | – |
| Akses internal ke data Terbatas/Rahasia | – | C | A | C | R | C | I | – |
| Berbagi eksternal (PKS + dokumen teknis) | A (risiko tinggi) | R | A (domain) | C | R (3scale/SFTP) | C | C | R |
| Aturan survivorship MDM | I | C | A | R | I | – | – | – |
| Ambang kualitas CDE | I | R | A | C | I | – | – | – |
| Kebijakan retensi & pemusnahan | A | R | C | C | R (eksekusi) | C | C | – |
| Perubahan skema sumber berdampak ESDP | I | C | A | C | R | – | – | – |

## 9. Tata kelola berbagi data eksternal

**Prinsip.** Eligibility ditentukan oleh **PKS dan dokumen teknis** yang disepakati antarinstansi (D4), sejalan dengan isi minimum PKS (Ps. 71(1), hlm. 56). Data Hub di DMZ, B2B, melalui 3scale (API REST/JSON) atau SFTP (D4) menjadi satu-satunya jalur downstream eksternal; akses DB langsung dihentikan bertahap ([[12-Integrasi]]).

**Siklus hidup perjanjian berbagi (usulan):**
1. **Permintaan** — masuk satu pintu (hub); cek apakah data sudah tersedia untuk unit lain (mengatasi permintaan berulang, R1 [S 00:13:22–00:16:35]).
2. **Penilaian** — tujuan, dasar hukum, skema (Ps. 69), kriteria mitra (Ps. 70), klasifikasi & label PDP, minimisasi elemen, DPIA bila perlu.
3. **Persetujuan** — data owner + OKI (Ps. 66(1)) + pejabat PDP; forum untuk risiko tinggi.
4. **Perjanjian** — PKS (Ps. 71) + dokumen teknis (elemen, kanal, freshness, kuota, retensi di sisi mitra, pemusnahan, audit). Dokumen teknis **tidak boleh** memuat nilai rahasia.
5. **Provisioning** — produk data di serving/gold → Data Hub → 3scale *application plan* per PKS.
6. **Operasi & monitoring** — log per transaksi, kuota, insiden; laporan berkala ke Unit Pemrakarsa (Ps. 73e–f).
7. **Reviu** — valuasi minimal 2 tahunan (Ps. 72(4)); kepatuhan pihak ketiga (Ps. 76(5)).
8. **Pengakhiran** — cabut kredensial dan plan, konfirmasi pemusnahan salinan di mitra.

**Kasus Kemnaker (D5) sebagai uji siklus.**

| Temuan D5 | Tahap siklus yang gagal | Dampak tata kelola | Kontrol tata kelola yang dibutuhkan |
|---|---|---|---|
| Akses DB link langsung | 4–5 | Hak akses lebih luas dari elemen PKS; melewati 3scale | Hanya Data Hub; elemen = dokumen teknis |
| Salinan lokal data di sisi Kemnaker | 4, 8 | Tidak ada retensi, pembatasan tujuan, pemusnahan | Klausul retensi & pemusnahan di PKS/dokumen teknis; hak audit |
| Masking satu arah | 2, 4 | Data pribadi dari mitra masuk tanpa minimisasi; dasar pemrosesan tidak jelas | Aturan minimisasi dua arah; label PDP di katalog |
| Kredensial teks biasa di `.env` | 4–5 | Dokumen teknis menjadi vektor akses | Larangan nilai rahasia di dokumen; pemilik secret dan rotasi |

Status adendum PKS Kemnaker terhadap Ps. 78 belum diketahui [perlu konfirmasi].

## 10. Stewardship MDM & CDE
- **Golden record** (D3-03): lapisan Data Stewardship menaungi DQ dan pencocokan. Usulan: steward bisnis per entitas (Peserta/TK, Pemberi Kerja/BU, Kantor, Program, Kanal) memegang antrean *match review*, keputusan survivorship, dan kasus merger (saat ini kasus per kasus, R1 [P 01:06:06–01:08:42]).
- **CDE**: data owner menetapkan CDE dan ambang; steward triase isu; hub memelihara daftar CDE di katalog ([[15-Metadata-Kualitas]] KD-2).
- **Parameter wajib:** daftar entitas master & steward-nya; SLA keputusan antrean; aturan eskalasi ke owner/forum; frekuensi reviu master data (D1: minimal setahun sekali, hlm. 89).

## 11. Manajemen isu & eskalasi
Alur: deteksi (quality gate, profiling, pengguna, mitra, audit) → registrasi isu data (terpisah dari tiket insiden TI, tetapi memakai tool ITSM yang sama [perlu konfirmasi tool]) → triase steward → owner domain → forum TKD (lintas domain atau risiko tinggi) → KPTI → Direksi (Ps. 7(1)e). Insiden yang menyangkut data pribadi diteruskan ke pejabat PDP dan OKI (Ps. 64).

**Parameter wajib (tanpa nilai):** kategori isu (definisi, kualitas, akses, berbagi, PDP, retensi); prioritas; SLA triase/penyelesaian per prioritas; ambang eskalasi; pemilik penutupan.

## 12. Metrik tata kelola (parameter saja)
1. Cakupan peran: % domain DBB dengan owner dan steward ditunjuk.
2. Cakupan katalog: % dataset ESDP dengan owner, steward, klasifikasi, dan label PDP tercatat.
3. Cakupan CDE: % CDE dengan ambang dan skor kualitas aktual.
4. Isu data: jumlah terbuka, umur, % selesai dalam SLA.
5. Berbagi eksternal: % pertukaran via Data Hub/3scale; % PKS dengan dokumen teknis lengkap; % PKS diadendum sesuai Ps. 78; jumlah akses DB langsung tersisa.
6. Retensi: % kelas data dengan jadwal retensi; % salinan mitra dengan klausul pemusnahan.
7. Kepatuhan: temuan audit/monev terkait data; tindak lanjut dalam SLA.
8. Aktivitas forum: keputusan tercatat per periode; backlog keputusan.

Target, frekuensi, dan pemilik tiap metrik ditetapkan oleh forum TKD.

## 13. Kepemilikan kebijakan retensi
- **Pemilik kebijakan:** data owner per domain (A), disusun hub (R), disahkan forum TKD, dieksekusi custodian; pejabat PDP dan Hukum dikonsultasi.
- **Dasar:** D1 tidak mengatur retensi maupun minimisasi; PP 33/2026 Ps. 75 mewajibkan dokumen kebijakan masa retensi dan Ps. 59 minimisasi [perlu konfirmasi]; data peserta berumur 40+ tahun (R1 [P 00:39:35–00:39:49]).
- **Parameter wajib:** kelas retensi per kategori data; pemicu awal masa retensi; tahapan aktif → arsip → pemusnahan per zona ESDP; perlakuan salinan di mitra; bukti pemusnahan; pengecualian (sengketa, audit, hukum).

## 14. Penautan tooling

| Tool | Fungsi tata kelola | Catatan |
|---|---|---|
| Dataedo (katalog bisnis) | Register owner/steward, glossary, CDE, klasifikasi, tautan PKS & kontrak data | Saat ini pasif (R1); perlu alur persetujuan [perlu konfirmasi fitur workflow] |
| Apache Atlas | Lineage teknis, tag klasifikasi untuk Ranger | Lineage belum ada (D3-01 merah) |
| Ranger | Penegakan akses berbasis tag ([[16-Keamanan-Data]] Opsi C) | Owner menyetujui, custodian menjalankan |
| Red Hat 3scale | Penegakan eligibility PKS: satu *application plan* per PKS, kuota, log | D4; D3-01 API Management merah |
| ITSM / register isu | Isu data dan keputusan forum | Tool belum diketahui [perlu konfirmasi] |

## 15. Keterkaitan dengan Blueprint
- **3.1–3.3:** bagian 2–4. **3.4 / 5.3–5.4:** bagian 6–14.
- **6 (roadmap) — usulan tema "Tata kelola sebagai fondasi interoperabilitas":**
  - **Tahun 1 — Fondasi:** juknis peran & hak keputusan; bentuk forum TKD; tunjuk owner/steward untuk 2–3 domain pilot; register PKS dan dokumen teknis standar; remediasi Kemnaker (hentikan DB link, klausul retensi); kebijakan retensi versi awal sebelum PP 33/2026 berlaku [perlu konfirmasi]; owner & steward tercatat di Dataedo.
  - **Tahun 2 — Layanan:** perluasan ke semua shared dimension dan domain DBB; siklus PKS penuh via Data Hub/3scale; manajemen isu data ber-SLA; metrik tata kelola dilaporkan ke KPTI.
  - **Tahun 3 — Optimasi:** delegasi penuh ke domain; tata kelola otomatis di katalog (alur persetujuan, policy-as-code); evaluasi kematangan dan revisi D1 untuk memasukkan peran, retensi, dan minimisasi.

## 16. Pertanyaan terbuka untuk BPJS
1. Apakah ada SK/SOP yang mendefinisikan data owner, data steward, dan data custodian pengganti Perdir 2015 yang dicabut?
2. Apa mandat dan program kerja Asdep Data Governance saat ini? Apakah struktur D2 hlm. 51 masih berlaku?
3. Apakah KPTI pernah memutus isu data? Mohon risalah 2 rapat terakhir.
4. Siapa yang berwenang menyetujui perubahan elemen data dan definisi lintas domain?
5. Bagaimana alur PKS berjalan dalam praktik, dan apakah register PKS ada? Status adendum Ps. 78?
6. Apakah Perdir PDP sudah terbit dan pejabat PDP sudah ditunjuk?
7. Siapa yang akan memiliki kebijakan retensi, dan bagaimana persiapan menghadapi PP 33/2026?

## 17. Poin diskusi (ikpu)
- [ ] Sepakati Opsi B (federasi hub-and-spoke bertahap) dengan hub di UMD/Asdep Data Governance.
- [ ] Sepakati definisi peran owner/steward/custodian dan forum TKD sebagai sub-forum KPTI; tetapkan instrumennya (juknis Ps. 3(2)).
- [ ] Tetapkan domain pilot Tahun 1 dan penunjukan owner/steward-nya.
- [ ] Sepakati siklus hidup PKS + dokumen teknis sebagai dasar eligibility Data Hub/3scale, dengan Kemnaker sebagai kasus remediasi pertama.
- [ ] Tetapkan pemilik kebijakan retensi dan minimisasi sebelum PP 33/2026 berlaku.
- [ ] Validasi skor bertanda [perlu konfirmasi], terutama TK-3, TK-9, dan TK-11, bersama tim data governance.

## Lampiran — Deskripsi diagram (gaya Excalidraw)
Diagram hub-and-spoke: di tengah lingkaran **Hub — Kantor Tata Kelola Data (UMD/Asdep DG)** dengan label "kebijakan · standar · katalog · metrik · register PKS". Di atas hub: kotak **Forum TKD** → panah eskalasi ke **KPTI → Direksi**. Mengelilingi hub: lima spoke domain (**Kepesertaan, Pemberi Kerja, Pelayanan/Klaim, Keuangan/Iuran, Investasi**), masing-masing berisi ikon *Owner* dan *Steward*. Di bawah: pita **Custodian (Ops TI, platform ESDP, 3scale)**. Di samping kanan: jalur **Berbagi eksternal** dengan delapan tahap siklus PKS menuju gerbang **Data Hub (DMZ) → 3scale/SFTP → Mitra**; jalur DB link Kemnaker digambar putus-putus dengan tanda silang. Kotak samping: **Pejabat PDP** dan **OKI** terhubung ke Forum TKD dan tahap persetujuan.
