---
title: "Tahap Persiapan"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 1 Tahap Persiapan

## 1.1 Persiapan di AWS RDS / Aurora

Beberapa konfigurasi yang butuh dilakukan di master database diantaranya:

- **Akses user**. Butuh dibuatkan user dengan spesifikasi sebagai berikut:
	- username: replication_backup
	- password: [minimal 16 karakter, isi hanya huruf, angka, atau _ - . ~]
	- role: rds_replication dan rds_superuser
- **Network**.
	- Izinkan network akses dari 10.0.70.0/25 ke database dengan port-nya (default: 5432)
	- Nama Endpoint/Hostname untuk akses database.
- **Konfigurasi Database**.
	- Setting Parameter group: rds.logical_replication = 1.
	- Setting Parameter group: shared_preload_libraries memuat pglogical.
	- Butuh Reboot instance jika rds.logical_replication dan shared_preload_libraries baru diubah.
- ~~Buat CloudWatch alarm untuk OldestReplicationSlotLag, TransactionLogsDiskUsage dan FreeStorageSpace~~
- ~~Setting max_replication_slots, max_wal_senders dan max_worker_processes cukup untuk semua subscription~~
- ~~Setting max_slot_wal_keep_size supaya storage master tidak penuh kalau replica mati~~
- ~~Jalankan CREATE EXTENSION pglogical di setiap database yang direplikasi~~
 
## 1.2 Persiapan VM
Persiapan di VM tempat Replica jalan diantaranya:
- VM dibuat di cluster Replica Database dengan Range 10.0.70.0/25 
- Buat partisi terpisah untuk `/data/postgresql` dan `/data/pg_wal`
- Total hardisk sama dengan total disk yang digunakan di master
- Pg_wal 10% s.d. 25% dari total data
- OS User dengan akses sudoers
