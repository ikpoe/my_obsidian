---
title: "Tahap Monitoring dan Backup (rencana)"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# Tahap Monitoring dan Backup (rencana)

## Monitoring replikasi di Grafana (rencana)

Belum ditulis. Rencana isi: lag slot di master, status subscription di replica, dan alarm CloudWatch. \<ISI: data source dan dashboard Grafana>

## Membuat user monitoring

User monitoring dipakai Grafana untuk membaca statistik replikasi, jadi user ini cukup diberi akses baca saja (least privilege). Beberapa hal yang perlu diperhatikan diantaranya:

pg_monitor sudah mencakup pg_read_all_stats, pg_read_all_settings dan pg_stat_scan_tables, jadi tidak perlu grant tambahan untuk baca statistik dan setting

Jangan berikan pg_read_all_data, SUPERUSER, REPLICATION atau rds_superuser ke user monitoring

CONNECTION LIMIT 5 dan statement_timeout 10s supaya query dashboard tidak membebani database

Akses ke pglogical cukup USAGE dan SELECT di schema pglogical

Password user monitoring diisi di \<ISI> dan hanya disimpan di data source Grafana

Berikut adalah command untuk membuat user monitoring di replica.

[[M0]]

Hasil yang diterima: CREATE ROLE, GRANT ROLE, GRANT, ALTER ROLE, GRANT dan GRANT tanpa error.

Catatan: jika ada tabel baru di schema pglogical (misalnya setelah upgrade extension), maka GRANT SELECT ON ALL TABLES IN SCHEMA pglogical perlu dijalankan ulang.

Berikut adalah command untuk menambahkan akses Grafana di pg_hba.conf dan reload konfigurasi.

[[M1]]

Hasil yang diterima: pg_reload_conf mengembalikan t, dan pg_hba_file_rules menampilkan rule monitoring tanpa error.

Catatan: jika IP Grafana juga masuk subnet di rule host all all yang sudah ada, maka rule tersebut yang terpakai lebih dulu karena pg_hba.conf dibaca dari atas ke bawah. Pindahkan rule monitoring ke atasnya kalau akses monitoring mau dibatasi ke \<DB_NAME> saja.

Berikut adalah command untuk verifikasi login sebagai user monitoring.

[[M2]]

Hasil yang diterima: current_user monitoring, query pg_stat_replication dan pg_replication_slots jalan tanpa permission denied, dan pglogical.subscription menampilkan subscription.

Catatan: jika pg_stat_replication kosong di replica, maka itu normal karena replica tidak punya walsender. Lag slot dipantau dari pg_replication_slots di master.

Catatan: jika database master ada di RDS/Aurora, maka GRANT pg_monitor ke user monitoring dijalankan oleh master user, dan metric storage (FreeStorageSpace, TransactionLogsDiskUsage) diambil dari CloudWatch, bukan dari user monitoring.

## Backup replica ke S3 (rencana)

Belum ditulis. \<ISI: tool backup, bucket S3, jadwal dan retensi>
