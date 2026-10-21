---
type: analysis
project: BPJSTK-Interoperabilitas-Data
blueprint: [2.1, 2.3]
status: draft-diskusi
tags: [metodologi, maturitas]
---
# Langkah 1 — Lensa Penilaian

> Acuan internal: 11 knowledge area DMBOK v2. Di laporan disebut "area pengelolaan data".

## A. Area Pengelolaan Data

| #   | Area (istilah laporan)              | Acuan internal                      | Lingkup | Alasan                                                                                                                                                                                                                   | Prioritas sesi |
| --- | ----------------------------------- | ----------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- |
| 1   | Tata Kelola Data                    | Data Governance                     | Ya      | Use case 3 TOR; Perdir 4/2025                                                                                                                                                                                            | 7              |
| 2   | Arsitektur Data                     | Data Architecture                   | Ya      | Inti Blueprint dan arsitektur target                                                                                                                                                                                     | 3              |
| 3   | Pemodelan & Desain Data             | Data Modeling & Design              | Ya      | Medallion (bronze/silver/gold) sebagai desain lapisan ESDP; pemodelan DWH (dimensional/Kimball, Data Vault, Inmon); kamus data 11 atribut Perdir; dasar data contract                                                    | 4              |
| 4   | Penyimpanan & Operasional Data      | Data Storage & Operations           | Ya      | Use case 1 (infra ESDP); backup/DR                                                                                                                                                                                       | 8              |
| 5   | Keamanan & Pelindungan Data Pribadi | Data Security                       | Ya      | UU PDP, PP 33/2026, klasifikasi                                                                                                                                                                                          | 6              |
| 6   | Integrasi & Interoperabilitas Data  | Data Integration & Interoperability | Ya      | Inti TOR; SMILE sebagai SPOF                                                                                                                                                                                             | 2              |
| 7   | Dokumen & Konten                    | Document & Content Mgmt             | Ringan  | Hanya yang relevan: definisi data lifecycle (buat, simpan, pakai, arsip, musnah), retensi & arsip (UU Kearsipan), dokumen pertukaran dengan mitra (lampiran klaim, biometrik), dokumen berisi data pribadi (scan KTP/KK) | 8              |
| 8   | Master & Reference Data             | Reference & Master Data             | Ya      | Gap terbesar (manual); batas SoR/MDM                                                                                                                                                                                     | 1              |
| 9   | Data Warehouse & BI                 | DW & BI                             | Ya      | Data lake/DWH di satu environment Cloudera                                                                                                                                                                               | 9              |
| 10  | Metadata                            | Metadata Mgmt                       | Ya      | Dataedo vs Atlas terpisah                                                                                                                                                                                                | 5              |
| 11  | Kualitas Data                       | Data Quality                        | Ya      | Dimensi kualitas di Perdir; rekonsiliasi                                                                                                                                                                                 | 5              |

> Usulan: Metadata & Kualitas Data dibahas dalam satu sesi. Dokumen & Konten dibahas bersama Penyimpanan & Operasional (sesi 8) karena sama-sama menyangkut lifecycle, retensi, dan arsip.
> Pembagian: Pemodelan & Desain Data membahas *bagaimana data dimodelkan dan dilapis* (medallion, pola DWH). DW & BI membahas *pemanfaatan dan penyajian* (data mart, BI, self-service, environment).

## B. Skala Maturitas (1–5)

| Level | Nama                  | Ciri                                                              |
| ----- | --------------------- | ----------------------------------------------------------------- |
| 1     | Awal (Initial)        | Ad hoc, bergantung individu, tidak terdokumentasi                 |
| 2     | Berulang (Repeatable) | Praktik ada di sebagian unit, belum standar                       |
| 3     | Terdefinisi (Defined) | Kebijakan, standar, dan peran formal; dijalankan konsisten        |
| 4     | Terkelola (Managed)   | Diukur dengan KPI, dipantau, ada tool pendukung                   |
| 5     | Optimal (Optimized)   | Perbaikan berkelanjutan, otomatis, terintegrasi lintas organisasi |

Aturan penilaian (usulan):
- Setiap skor wajib punya bukti (D1/D2 + halaman, atau hasil wawancara).
- Bedakan **kebijakan** (Perdir) dan **praktik** (EA as-is). Kebijakan tanpa praktik tidak menaikkan skor di atas 2.
- Setiap area punya skor as-is dan target tahun 1–3.

## C. Template Per Area
Dipakai untuk `Area-XX-<Nama>.md`:

| Aspek | Praktik yang diharapkan | Kebijakan BPJS (D1) | Kondisi saat ini (D2) | Gap | Skor |
|---|---|---|---|---|---|

## D. Poin Diskusi
- [ ] Setuju dengan daftar area dalam lingkup?
- [ ] Setuju urutan sesi (Master Data → Integrasi → Arsitektur → Pemodelan & Desain (medallion, DWH) → Metadata & Kualitas → Keamanan → Tata Kelola → Penyimpanan + Dokumen & Konten (data lifecycle) → DW & BI)?
- [ ] Skala 1–5 dan aturan penilaian OK?

## E. Catatan Terbuka (Parking Lot)
- **Guideline pemilihan tools & deployment (on-premise / cloud / hybrid).** Sertakan komparasi dan saran. Usulan penempatan: bab baru *5.7 Panduan Pemilihan Teknologi & Model Deployment* di Blueprint, dirujuk oleh use case 7.1 dan roadmap. Bahan awal:
  - Kriteria: regulasi (PP 71/2019, UU PDP & PP 33/2026, kebijakan PDN/PDNS, lokasi data), keamanan, TCO, skalabilitas, kematangan SDM, vendor lock-in, investasi eksisting (Cloudera, Talend, Qlik, Oracle).
  - Komparasi opsi: on-premise vs private cloud vs public cloud (region Indonesia) vs hybrid, per lapisan ESDP.
  - Pembelajaran K/L yang sudah memakai cloud, dan risiko insiden PDNS Juni 2024.
  - Bentuk penyajian: matriks keputusan berbobot dan decision tree.
