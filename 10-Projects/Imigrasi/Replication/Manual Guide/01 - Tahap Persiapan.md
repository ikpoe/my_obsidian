---
title: "Tahap Persiapan"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 1 Tahap Persiapan

## 1.1 Persiapan di AWS RDS / Aurora

Beberapa konfigurasi yang butuh dilakukan di master database diantaranya:

- Izinkan  network akses dari 10.0.70.0/25 ke database dengan port-nya (default: 5432)
- Minta Endpoint/Hostname untuk akses database.
- Buat CloudWatch alarm untuk OldestReplicationSlotLag, TransactionLogsDiskUsage dan FreeStorageSpace
- Membuat user `replication_backup` dengan role `rds_replication` dan `rds_superuser`
- Setting Parameter group: rds.logical_replication = 1.
- Setting Parameter group: shared_preload_libraries memuat pglogical.
- Setting Parameter group: output_plugin_libraries memuat pglogical_output (PostgreSQL 18.6). Tanpa ini slot tidak bisa dibuat, cukup reload.
- Reboot instance setelah rds.logical_replication dan shared_preload_libraries diubah, kedua parameter ini baru aktif setelah reboot
- ~~Setting max_replication_slots, max_wal_senders dan max_worker_processes cukup untuk semua subscription~~
- ~~Setting max_slot_wal_keep_size supaya storage master tidak penuh kalau replica mati~~
- ~~Jalankan CREATE EXTENSION pglogical di setiap database yang direplikasi~~



- 
## 1.2 Persiapan VM
VM dibuat di cluster Replica Database dengan Range 10.0.70.0/25 
- Buat partisi terpisah untuk `/data/postgresql` dan `/data/pg_wal`
- Total hardisk sama dengan total disk yang digunakan di master
- Pg_wal 10% s.d. 25% dari total data
- OS User dengan akses sudoers
