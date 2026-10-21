---
title: "Tahap Operasional"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# Tahap Operasional

## Tabel tanpa identity

Beberapa hal yang perlu diperhatikan untuk tabel tanpa identity diantaranya:

- Cek ulang kelayakan setiap ada tabel baru (query di Tahap Replikasi Database)
- Tabel tanpa identity hanya boleh masuk set default_insert_only, dan hanya jika aplikasinya tidak pernah UPDATE atau DELETE
- Selain itu tabel tidak didaftarkan

## Slot replikasi, max_slot_wal_keep_size dan alarm

Slot pglogical di master menahan WAL selama replica belum menerima data. Jika replica mati lama, maka storage RDS bisa penuh dan master ikut berhenti. Pengamannya diantaranya:

- Setting max_slot_wal_keep_size di parameter group master: \<ISI: batas WAL dalam MB>
- Jika batas terlewati, maka slot menjadi lost dan replica harus sync ulang dari awal
- CloudWatch alarm OldestReplicationSlotLag: \<ISI: threshold>
- CloudWatch alarm TransactionLogsDiskUsage dan FreeStorageSpace: \<ISI: threshold>
- Alarm dikirim ke \<ISI: SNS topic / email / channel>

## Failover dan upgrade master

Beberapa hal yang perlu dicek saat failover atau upgrade master diantaranya:

- Slot logical bisa hilang saat failover Multi-AZ/Aurora atau major upgrade master \<ISI: cek dokumentasi AWS sesuai versi engine>
- Setelah failover atau upgrade, cek slot di master (query di Tahap Verifikasi)
- Jika slot hilang, maka replica di-sync ulang (lihat bagian berikut)
- Selalu pakai writer endpoint, bukan reader endpoint

## Replica mati lama

Langkah yang dilakukan tergantung kondisi slot di master, diantaranya:

- Slot masih ada dan wal_status bukan lost: start PostgreSQL lagi, replica akan mengejar sendiri
- Mati terencana lebih lama dari batas max_slot_wal_keep_size: drop subscription dulu supaya WAL di master tidak tertahan
- Slot dengan wal_status = lost atau slot hilang: sync ulang dari awal dengan command di bawah

Berikut adalah command untuk sync ulang dari awal.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB -c "SELECT pglogical.drop_subscription('$SUBSCRIPTION');"
sudo -u postgres $PGBIN/psql "$MASTER_DSN" -c "SELECT slot_name, active, wal_status FROM pg_replication_slots WHERE plugin = 'pglogical_output';"
# hanya jika slot pgl_... masih ada dan active = f:
sudo -u postgres $PGBIN/psql "$MASTER_DSN" -c "SELECT pg_drop_replication_slot('<ISI: slot_name pgl_...>');"
sudo -u postgres $PGBIN/dropdb $TARGET_DB
```

Setelah itu ulangi Tahap Replikasi Database mulai dari Buat database tujuan (ICU und), dengan pemisahan pre-data dan post-data. Provider node yang sudah ada di master tidak dibuat ulang.
