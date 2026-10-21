---
title: "Tahap Replikasi DDL"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# Tahap Replikasi DDL

DDL biasa di master dijalankan seperti biasa, tidak dibungkus fungsi apa pun. Event trigger di master menulis tiap DDL ke repl.ddl_queue di transaksi yang sama, lalu replica menjalankan baris kelas auto lewat trigger ENABLE ALWAYS. Skrip dipasang setelah subscription replicating, dengan urutan diantaranya:

- Replica dulu: ddl_apply.sql dari role platform.postgres.subscription (files/ddl_apply.sql)
- Lalu master: ddl_capture.sql dari role platform.postgres.provider (files/ddl_capture.sql)
- Jika urutannya kebalik, maka baris antrean tiba sebelum tabel ada di replica dan subscription berhenti

Berikut adalah command untuk memasang kedua skrip.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB -v ON_ERROR_STOP=1 -f <ISI: path files/ddl_apply.sql dari role platform.postgres.subscription>
sudo -u postgres $PGBIN/psql "$MASTER_DSN" -v ON_ERROR_STOP=1 -f <ISI: path files/ddl_capture.sql dari role platform.postgres.provider>
```

Berikut adalah kelas DDL dan perlakuannya di replica.

| Kelas | Contoh | Di replica |
|---|---|---|
| auto | CREATE TABLE, CREATE INDEX, CREATE VIEW, CREATE FUNCTION, CREATE SEQUENCE, ALTER TABLE ADD COLUMN nullable atau punya default, ALTER TYPE ADD VALUE | Dijalankan di dalam stream |
| approval | ALTER COLUMN TYPE, SET NOT NULL, RENAME, DROP CONSTRAINT, ADD COLUMN NOT NULL tanpa default | Dicatat di repl.ddl_queue, tidak dijalankan |
| never | DROP TABLE, DROP COLUMN, CREATE EXTENSION, perintah role | Dicatat, tidak dijalankan |
| deferred | CREATE INDEX CONCURRENTLY | Dicatat, tidak bisa jalan di dalam transaksi apply |
| ambiguous | Satu pesan klien berisi lebih dari satu statement | Dicatat, tidak dijalankan |

Beberapa hal yang perlu diketahui diantaranya:

- TRUNCATE tidak punya tag event trigger, jadi tidak tertangkap
- Statement auto yang gagal menghentikan seluruh subscription sampai diperbaiki (disengaja)
- Tabel baru tercipta di replica, tetapi tidak otomatis masuk replication set. Barisnya tidak mengalir sampai didaftarkan dengan replication_set_add_table
- Role aplikasi tidak ada di replica. Statement yang menyebut role (OWNER TO, GRANT, POLICY ... TO role) bisa gagal dan menghentikan subscription. Yang tidak menyebut role berjalan sebagai postgres

Berikut adalah command untuk mendaftarkan tabel baru ke replication set.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN" -c "SET lock_timeout = '3s'" -c "SELECT pglogical.replication_set_add_table('default', '<ISI: schema.tabel_baru>'::regclass, synchronize_data := true);"
```

Catatan: jika subscription berhenti karena statement auto gagal, maka cek log replica dan repl.ddl_queue, perbaiki penyebabnya, lalu subscription jalan lagi.
