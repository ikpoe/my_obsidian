---
type: source
kode: D6
judul: Prosedur Pengelolaan Kualitas Data
nomor: 4.009/SOP/MDT/082024
versi: "0.1"
pembuat: Bidang Data Governance
pemilik: Deputi Bidang Manajemen Data dan Analitik
klasifikasi: Dokumen Terbatas
halaman: 66
berkas: 4.009-SOP-Prosedur-Pengelolaan-Kualitas-Data.pdf (gitignored)
diterima: 2026-11-02
konvensi_sitasi: "D6 hlm. n = nomor halaman tercetak"
---

# D6 — Prosedur Pengelolaan Kualitas Data

> Dokumen terbatas. Catatan ini hanya memuat pola umum. Kriteria penanganan khusus, aturan validasi per elemen, nama modul aplikasi, dan contoh kueri tidak disalin.

## Ringkasan
SOP operasional yang menjabarkan siklus kualitas data: penetapan metrik → profiling → cleansing → penilaian → tindak lanjut, dengan fokus data peserta (building block Kepesertaan).

## Struktur
- hlm. 4–9: dasar hukum, tujuan, ruang lingkup, referensi (DAMA-DMBOK2, COBIT 2019 APO14), definisi, prinsip, dokumen terkait, ketentuan umum
- hlm. 10–13: garis besar prosedur dan teknik profiling
- hlm. 14–29: prosedur penetapan metrik, profiling, cleansing, penilaian (RACI per prosedur)
- hlm. 30–43: indeks kualitas data peserta dan tindak lanjut per tingkat
- hlm. 44–55: deteksi dan tindak lanjut inkonsistensi data
- hlm. 56–66: lampiran (kertas kerja CDE, skala, contoh pembobotan, kertas kerja metrik, template profiling/cleansing, laporan kualitas data)

## Poin penting
- Peran: Data Quality, Data Liaison, Business Owner (Unit Bisnis Kantor Pusat), Asdep Interoperability and Processing sebagai penyetuju (hlm. 5, 14–29).
- CDE kepesertaan berbobot dengan aturan per dimensi (hlm. 56–61); dimensi termasuk timeliness dan konsistensi lintas kanal (hlm. 62).
- Indeks tiga tingkat di batang tubuh (hlm. 30) vs lima grade di lampiran (hlm. 60) [perlu konfirmasi].
- Laporan kualitas data bulanan; platform data quality summary untuk cabang; perbaikan via permohonan layanan TI dan koreksi oleh cabang (hlm. 24–25, 29, 34–35).
- Penanganan khusus peserta non-aktif yang tidak terkonfirmasi, dengan SLA perubahan flag (hlm. 8–9, 38–39).
- Tindak lanjut prioritas rendah: dasar hukum merujuk Perdir 14/PERDIR.06/042022 yang dicabut D1 (hlm. 4; D1 hlm. 62).
