---
title: "Tahap Database Berikutnya dan Tabel Nilai"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 8 Tahap Database Berikutnya dan Tabel Nilai

Database berikutnya diulang dengan langkah yang sama. Beberapa hal yang perlu diperhatikan diantaranya:

- Jika di VM yang sama, maka Tahap Instalasi dan Tahap Konfigurasi tidak perlu diulang
- Env file baru dibuat dari env file yang sudah ada, lalu semua value diganti sesuai tabel di bawah
- Tambahkan baris master baru di /var/lib/pgsql/.pgpass
- Ulangi Tahap Replikasi Database, Tahap Verifikasi dan Tahap Replikasi DDL dengan env file yang baru
- Setiap database punya provider node, subscriber node dan subscription sendiri
- Pemisahan pre-data dan post-data berlaku untuk database berikutnya dan sync ulang

Berikut adalah command untuk membuat env file database berikutnya.

```bash
cp /root/replikasi/<SOURCE_ID lama>.env /root/replikasi/<SOURCE_ID>.env
vi /root/replikasi/<SOURCE_ID>.env
source /root/replikasi/<SOURCE_ID>.env
grep -n 'ISI:' /root/replikasi/<SOURCE_ID>.env
```

Hasil yang diterima: grep tidak menampilkan apa pun, dan SOURCE_ID sudah diganti supaya nama node dan subscription tidak bentrok dengan database lain.

Berikut adalah value untuk masing-masing database. Kolom APGAKUM adalah record setup pertama.

| Parameter | APGAKUM (setup pertama) | Dokim | Visa |
|---|---|---|---|
| VM replica | imi-prod-apgakum-database-replica | \<ISI> | \<ISI> |
| SOURCE_HOST (writer endpoint) | apgakum-production-prosia.cfss8sgys92h.ap-southeast-3.rds.amazonaws.com | \<ISI> | \<ISI> |
| SOURCE_DB / TARGET_DB | gakkum_01 | \<ISI> | \<ISI> |
| SOURCE_ID | apgakum | \<ISI> | \<ISI> |
| Node dan subscription | provider_apgakum, subscriber_apgakum, sub_apgakum | \<ISI> | \<ISI> |
| VM (vCPU, RAM) | 2 vCPU, 7.5 GiB | \<ISI> | \<ISI> |
| Memory (postgresql.conf) | shared_buffers = 2GB<br>effective_cache_size = 5GB<br>maintenance_work_mem = 256MB<br>max_connections = 100 | \<ISI> | \<ISI> |
| max_worker_processes | 16 | 16 | 16 |
| Catatan | Copy awal sudah jalan dengan index terpasang, jangan drop index untuk dibuat ulang.<br>Skrip DDL (ddl_apply.sql dan ddl_capture.sql) belum dipasang. | \<ISI> | \<ISI> |

Catatan: max_worker_processes = 16 tetap. Jika jumlah database di satu VM bertambah, maka jumlah worker yang terpakai perlu dicek lagi.
