---
title: "Tahap Replikasi Database"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---
 
# 4 Tahap Replikasi Database

Tahap ini diulang untuk setiap database yang direplikasi. Setiap database punya env file sendiri /root/replikasi/\<SOURCE_ID>.env dengan value dari tabel nilai. \$MASTER_DSN dipakai untuk sesi psql admin ke master dan sebagai provider_dsn di 4.10.3.

Variable di env file dibaca di dalam sesi psql dengan \getenv \<nama_psql> \<NAMA_ENV>, lalu dipakai sebagai :'nama_psql' untuk nilai teks atau :"nama_psql" untuk nama objek. Karena itu psql dijalankan dengan sudo -E supaya env dari bash ikut terbawa ke user postgres.

Catatan: jika \echo menampilkan nilai kosong, maka env file belum di-source atau psql dijalankan tanpa sudo -E. Keluar dari psql, jalankan source /root/replikasi/\<SOURCE_ID>.env, lalu login lagi.

## 4.1 Cek kelayakan tabel di master

Tabel dicek dulu sebelum didaftarkan ke replication set. Sebisa mungkin semua tabel dimasukkan, kecuali yang memang tidak bisa direplikasi. Cek tidak punya PK saja tidak cukup. Aturan kelayakan diantaranya:

- Ada primary key, atau replica identity unique index yang valid: masuk set default
- REPLICA IDENTITY FULL: tidak didaftarkan, karena pglogical tidak bisa mereplikasi tabel dengan REPLICA IDENTITY FULL
- Tanpa identity (tanpa PK dan tanpa replica identity index): tidak didaftarkan

Berikut adalah command untuk masuk ke psql di master. Sesi ini tetap dibuka sampai selesai 4.3.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql "$MASTER_DSN"
```

Berikut adalah command untuk mengecek identity semua tabel di semua schema di master.

```sql
SELECT n.nspname AS schema,
  c.relname AS table_name,
  EXISTS (SELECT 1 FROM pg_constraint k WHERE k.conrelid = c.oid AND k.contype = 'p') AS ada_pk,
  EXISTS (SELECT 1 FROM pg_index i WHERE i.indrelid = c.oid AND i.indisunique AND i.indisvalid AND NOT i.indisprimary) AS ada_unique_index,
  CASE c.relreplident
    WHEN 'd' THEN 'default'
    WHEN 'i' THEN 'index'
    WHEN 'f' THEN 'full'
    WHEN 'n' THEN 'nothing'
  END AS replica_identity,
  CASE
    WHEN c.relreplident = 'f' THEN 'tidak (REPLICA IDENTITY FULL)'
    WHEN c.relreplident = 'd' AND EXISTS (SELECT 1 FROM pg_constraint k WHERE k.conrelid = c.oid AND k.contype = 'p') THEN 'ya (default)'
    WHEN c.relreplident = 'i' AND EXISTS (SELECT 1 FROM pg_index i WHERE i.indrelid = c.oid AND i.indisreplident AND i.indisvalid) THEN 'ya (default)'
    ELSE 'tidak (tanpa identity)'
  END AS didaftarkan
FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE c.relkind IN ('r','p')
  AND n.nspname NOT IN ('pg_catalog','information_schema','pglogical')
  AND n.nspname NOT LIKE 'pg_toast%'
ORDER BY didaftarkan, 1, 2;
```

Hasil yang diterima diantaranya:

- ada_pk dan ada_unique_index bernilai t atau f
- replica_identity berisi default, index, full, atau nothing
- didaftarkan berisi ya (default) untuk tabel yang masuk set default di 4.3, dan tidak untuk REPLICA IDENTITY FULL atau tabel tanpa identity

Catatan: jika ada_pk = f tapi ada_unique_index = t, maka tabel bisa dibuat layak dengan ALTER TABLE \<TABLE> REPLICA IDENTITY USING INDEX \<UNIQUE_INDEX>, asalkan semua kolom index tersebut NOT NULL.

## 4.2 Buat provider node di master

### 4.2.1 CREATE EXTENSION pglogical

Extension pglogical dan provider node dibuat di master. DSN provider hanya host=\<SOURCE_HOST> port=5432 dbname=\<SOURCE_DB>, tanpa user dan tanpa password. Sebelum membuat extension, cek dulu versi pglogical yang tersedia di master. Versi ini harus sama dengan pglogical yang terpasang di replica (2.5.3).

```sql
SELECT name, default_version, installed_version FROM pg_available_extensions WHERE name = 'pglogical';
```

Hasil yang diterima: default_version sama dengan versi pglogical di replica.

Berikut adalah command untuk membuat provider node, dijalankan di sesi psql master yang sama.

```sql
CREATE EXTENSION IF NOT EXISTS pglogical;
SELECT extversion FROM pg_extension WHERE extname = 'pglogical';
```

Hasil yang diterima: extversion 2.4.6.

### 4.2.2 Provider node and sets

Berikut adalah command untuk membaca variable dari env file ke sesi psql. Variable yang dibutuhkan: PROVIDER_NODE, SOURCE_HOST, SOURCE_PORT, SOURCE_DB.

```sql
\getenv provider_node PROVIDER_NODE
\getenv source_host SOURCE_HOST
\getenv source_port SOURCE_PORT
\getenv source_db SOURCE_DB
\echo :provider_node :source_host :source_port :source_db
```

Hasil yang diterima: \echo menampilkan nilai setiap variable, tidak ada yang kosong.

Berikut adalah command untuk menjalankan SQL-nya.

```sql
SELECT pglogical.create_node(
  node_name := :'provider_node', 
  dsn := 'host=' || :'source_host' || ' port=' || :'source_port' || ' dbname=' || :'source_db');
```

Berikut adalah command untuk mengecek provider node dan replication set, di sesi psql master yang sama.

```sql
SELECT n.node_name, i.if_dsn
FROM pglogical.node n JOIN pglogical.node_interface i ON i.if_nodeid = n.node_id;
SELECT set_name FROM pglogical.replication_set ORDER BY 1;
```

Hasil yang diterima diantaranya:

- node_name sama dengan nilai :provider_node, dan if_dsn berisi host, port dan dbname master tanpa user dan password
- replication set berisi default, default_insert_only dan ddl_sql

Catatan: jika provider node sudah ada (mis. saat sync ulang), maka create_node tidak dijalankan lagi.


## 4.3 Daftarkan tabel satu per satu

Tabel tidak didaftarkan sekaligus per schema, karena itu mengunci semua tabel. Satu statement per tabel, autocommit, lock_timeout = 3s dan synchronize_data := false. Berikut adalah command untuk membuat daftar statement tabel yang layak masuk set default, lalu menjalankannya, di sesi psql master yang sama. \gexec menjalankan setiap baris hasil query sebagai statement terpisah (autocommit).

```sql
SET lock_timeout = '3s';
SELECT format('SELECT pglogical.replication_set_add_table(''default'', %L::regclass, synchronize_data := false);', c.oid::regclass::text)
FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE c.relkind IN ('r','p')
  AND n.nspname NOT IN ('pg_catalog','information_schema','pglogical','pg_toast')
  AND n.nspname NOT LIKE 'pg_temp%' AND n.nspname NOT LIKE 'pg_toast_temp%'
  AND ((c.relreplident = 'i' AND EXISTS (SELECT 1 FROM pg_index i WHERE i.indrelid = c.oid AND i.indisreplident AND i.indisvalid))
    OR (c.relreplident = 'd' AND EXISTS (SELECT 1 FROM pg_constraint k WHERE k.conrelid = c.oid AND k.contype = 'p')))
  AND NOT EXISTS (SELECT 1 FROM pglogical.replication_set_table rt WHERE rt.set_reloid = c.oid)
ORDER BY 1 \gexec
```

Hasil yang diterima: setiap statement mengembalikan t.

Catatan: jika ada statement yang gagal karena lock timeout, maka jalankan lagi query yang sama. Tabel yang sudah masuk set tidak muncul lagi di hasil query.

Catatan: jika schema pglogical ikut terdaftar di set, maka initial copy di replica gagal dengan error duplicate key "node_pkey". Schema sistem (pg_catalog, information_schema, pglogical, pg_toast) tidak boleh masuk replication set.

Tabel tanpa identity yang aplikasinya tidak pernah UPDATE atau DELETE didaftarkan manual ke set default_insert_only. Berikut adalah command untuk satu tabel, di sesi psql master yang sama.

```sql
SET lock_timeout = '3s';
SELECT pglogical.replication_set_add_table('default_insert_only', '<ISI: schema.tabel>'::regclass, synchronize_data := false);
\q
```

## 4.4 Readiness?

Langkah ini memastikan semua step di master (4.1 sampai 4.3) sudah dilakukan sebelum lanjut ke replica. Berikut adalah command untuk mengecek kesiapan, dijalankan di sesi psql master.

```sql
SELECT
  (SELECT extversion FROM pg_extension WHERE extname = 'pglogical') AS pglogical_version,
  (SELECT string_agg(node_name, ',') FROM pglogical.node) AS provider_node,
  (SELECT count(*) FROM pglogical.tables WHERE set_name = 'default') AS tabel_di_set_default,
  (SELECT count(*)
     FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
    WHERE c.relkind IN ('r','p')
      AND n.nspname NOT IN ('pg_catalog','information_schema','pglogical')
      AND n.nspname NOT LIKE 'pg_toast%'
      AND ((c.relreplident = 'i' AND EXISTS (SELECT 1 FROM pg_index i WHERE i.indrelid = c.oid AND i.indisreplident AND i.indisvalid))
        OR (c.relreplident = 'd' AND EXISTS (SELECT 1 FROM pg_constraint k WHERE k.conrelid = c.oid AND k.contype = 'p')))
      AND NOT EXISTS (SELECT 1 FROM pglogical.replication_set_table rt WHERE rt.set_reloid = c.oid)) AS tabel_layak_belum_didaftarkan;
```

Hasil yang diterima:

- pglogical_version = 2.4.6
- provider_node berisi \<PROVIDER_NODE>
- tabel_di_set_default sama dengan jumlah tabel berstatus ya (default) di 4.1
- tabel_layak_belum_didaftarkan = 0

Catatan: jika tabel_layak_belum_didaftarkan lebih dari 0, maka jalankan lagi query di 4.3.

## 4.5 Buat database Replika (libc en_US.UTF-8)

Database tujuan dibuat kosong dari template0 dengan locale yang sama dengan master, yaitu provider libc dengan Collate dan Ctype en_US.UTF-8. Cek locale database di master dengan \l, kolom Locale memang kosong untuk provider libc. Nama database diambil dari env TARGET_DB. Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql
```

Berikut adalah command untuk membaca variable dari env file ke sesi psql. Variable yang dibutuhkan: TARGET_DB.

```sql
\getenv target_db TARGET_DB
\echo :target_db
```

Hasil yang diterima: \echo menampilkan nilai setiap variable, tidak ada yang kosong.

Berikut adalah command untuk menjalankan SQL-nya.

```sql
CREATE DATABASE :"target_db" TEMPLATE template0 ENCODING 'UTF8'
  LOCALE_PROVIDER libc
  LC_COLLATE 'en_US.UTF-8' LC_CTYPE 'en_US.UTF-8';
SELECT datlocprovider, datcollate, datctype FROM pg_database WHERE datname = :'target_db';
\q
```

Hasil yang diterima: c | en_US.UTF-8 | en_US.UTF-8.

Catatan: locale tidak bisa diubah setelah ada objek di database. Jika hasilnya bukan c | en_US.UTF-8 | en_US.UTF-8, maka drop database dan buat ulang sebelum restore schema.

Catatan: jika CREATE DATABASE gagal dengan invalid locale name "en_US.UTF-8", maka install dulu glibc-langpack-en dengan dnf install -y glibc-langpack-en, lalu ulangi.

Catatan: collation libc mengikuti versi glibc OS. Jika glibc di VM di-upgrade major (mis. upgrade AlmaLinux major), maka jalankan REINDEX untuk index bertipe text.

## 4.6 Dump struktur pre-data dan post-data

Schema tidak di-restore utuh sebelum subscription, karena index sekunder memperlambat initial copy. Struktur di-dump jadi dua file. Berikut adalah command untuk dump struktur dari master.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/pg_dump "$MASTER_DSN" --schema-only --no-owner --no-privileges \
  --section=pre-data \
  --exclude-schema=pglogical --exclude-schema=pglogical_origin \
  --exclude-extension=pglogical \
  -f /var/lib/pgsql/pre_$SOURCE_ID.sql
sudo -u postgres $PGBIN/pg_dump "$MASTER_DSN" --schema-only --no-owner --no-privileges \
  --section=post-data \
  --exclude-schema=pglogical --exclude-schema=pglogical_origin \
  --exclude-extension=pglogical \
  -f /var/lib/pgsql/post_$SOURCE_ID.sql
ls -l /var/lib/pgsql/pre_$SOURCE_ID.sql /var/lib/pgsql/post_$SOURCE_ID.sql
```

## 4.7 Restore pre-data dan primary key

Sebelum create_subscription, pre_\$SOURCE_ID.sql di-restore utuh. Dari post_\$SOURCE_ID.sql hanya ADD CONSTRAINT ... PRIMARY KEY dan index replica identity yang dipasang. Primary key tidak boleh ditunda, karena tanpa PK subscription down setelah copy. Berikut adalah command untuk memisahkan primary key.

```bash
source /root/replikasi/<SOURCE_ID>.env
awk 'BEGIN{RS=";\n"} /PRIMARY KEY/ {print $0 ";"}' /var/lib/pgsql/post_$SOURCE_ID.sql > /var/lib/pgsql/pk_$SOURCE_ID.sql
grep -n "REPLICA IDENTITY USING INDEX" /var/lib/pgsql/post_$SOURCE_ID.sql
vi /var/lib/pgsql/pk_$SOURCE_ID.sql
```

Di vi, tambahkan CREATE UNIQUE INDEX dan ALTER TABLE ... REPLICA IDENTITY USING INDEX untuk tabel yang muncul di hasil grep.

Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk membaca variable dari env file ke sesi psql. Variable yang dibutuhkan: SOURCE_ID.

```sql
\getenv source_id SOURCE_ID
\set pre_file /var/lib/pgsql/pre_ :source_id .sql
\set pk_file /var/lib/pgsql/pk_ :source_id .sql
\echo :pre_file :pk_file
```

Hasil yang diterima: \echo menampilkan nilai setiap variable, tidak ada yang kosong.

Berikut adalah command untuk restore pre-data lalu primary key di sesi psql replica tersebut.

```sql
\set ON_ERROR_STOP on
\i :pre_file
\i :pk_file
```

Berikut adalah command untuk mengecek hasil restore, di sesi psql replica yang sama.

```sql
SELECT count(*) AS jumlah_tabel FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE c.relkind IN ('r','p') AND n.nspname NOT IN ('pg_catalog','information_schema','pglogical') AND n.nspname NOT LIKE 'pg_toast%';
SELECT count(*) AS jumlah_pk FROM pg_constraint WHERE contype = 'p';
SELECT relname, n_live_tup FROM pg_stat_user_tables WHERE n_live_tup > 0;
\q
```

Hasil yang diterima diantaranya:

- jumlah_tabel sama dengan jumlah tabel di 4.1
- jumlah_pk sama dengan jumlah tabel ada_pk = t di 4.1
- query terakhir tidak mengembalikan baris, semua tabel masih kosong

Catatan: jika restore error karena extension khusus RDS (mis. aws_s3, aws_commons), maka drop database, buat ulang dengan libc en_US.UTF-8, tambahkan --exclude-extension=\<nama extension> di pg_dump, lalu ulangi.

## 4.8 Roles from the master

Catatan: Role dan grant master tidak disalin. Role untuk user yang membaca replica: \<ISI: daftar user/role read-only>.

## 4.9 Cek sesi psql di replica

Sebelum subscription dibuat, sesi psql harus benar-benar di replica. Jika sesi ternyata di master, maka master berlangganan ke dirinya sendiri. Berikut adalah command untuk masuk ke psql di replica. Sesi ini tetap dibuka sampai selesai 4.10.3.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk mengecek sesi.

```sql
SELECT inet_server_addr(), current_user;
```

Hasil yang diterima: inet_server_addr kosong dan current_user postgres.

## 4.10 Buat subscriber node dan subscription di replica

Subscriber node memakai socket Unix host=/var/run/postgresql. Subscription dibuat dengan forward_origins := '{}', lalu pglogical langsung melakukan initial copy data dari master. Berikut adalah command untuk membuat subscription.

### 4.10.1 CREATE EXTENSION pglogical

```sql
CREATE EXTENSION IF NOT EXISTS pglogical;
SELECT extversion FROM pg_extension WHERE extname = 'pglogical';
```

Hasil yang diterima: extversion 2.4.6, sama dengan di master.

### 4.10.2 Subscriber node

Berikut adalah command untuk membaca variable dari env file ke sesi psql. Variable yang dibutuhkan: SUBSCRIBER_NODE, TARGET_DB.

```sql
\getenv subscriber_node SUBSCRIBER_NODE
\getenv target_db TARGET_DB
\echo :subscriber_node :target_db
```

Hasil yang diterima: \echo menampilkan nilai setiap variable, tidak ada yang kosong.

Berikut adalah command untuk menjalankan SQL-nya.

```sql
SELECT pglogical.create_node(node_name := :'subscriber_node', dsn := 'host=/var/run/postgresql port=5432 dbname=' || :'target_db');
```

Berikut adalah command untuk mengecek subscriber node, di sesi psql replica yang sama.

```sql
SELECT n.node_name, i.if_dsn
FROM pglogical.local_node l
JOIN pglogical.node n ON n.node_id = l.node_id
JOIN pglogical.node_interface i ON i.if_id = l.node_local_interface;
```

Hasil yang diterima: node_name sama dengan nilai :subscriber_node, dan if_dsn berisi host=/var/run/postgresql dengan dbname replica.

### 4.10.3 create_subscription

Berikut adalah command untuk membaca variable dari env file ke sesi psql. Variable yang dibutuhkan: SUBSCRIPTION, MASTER_DSN.

```sql
\getenv subscription SUBSCRIPTION
\getenv master_dsn MASTER_DSN
\echo :subscription :master_dsn
```

Hasil yang diterima: \echo menampilkan nilai setiap variable, tidak ada yang kosong.

Berikut adalah command untuk menjalankan SQL-nya.

```sql
SELECT pglogical.create_subscription(
  subscription_name := :'subscription',
  provider_dsn := :'master_dsn',
  replication_sets := ARRAY['default', 'default_insert_only'],
  synchronize_structure := false,
  synchronize_data := true,
  forward_origins := '{}'
);
```

Berikut adalah command untuk mengecek subscription, di sesi psql replica yang sama.

```sql
SELECT subscription_name, status, provider_node, replication_sets
FROM pglogical.show_subscription_status();
\q
```

Hasil yang diterima: subscription_name sama dengan nilai :subscription, dan status initializing (initial copy sedang berjalan) atau replicating.

Catatan: jika log replica berisi nonrecoverable step, maka drop subscription di replica, drop slot pgl_... di master hanya jika active = f, buat database tujuan dari kosong, lalu ulangi dari Dump struktur pre-data dan post-data. Jangan buat subscription kedua dengan nama yang sama.


## 4.11 Pantau progres initial copy

Selama initial copy, keadaan yang normal diantaranya:

- status = initializing
- local_sync_status berisi satu baris dengan nama tabel kosong dan sync_status = d
- Slot di master active = f
- count(\*) di replica tetap 0, karena copy berjalan dalam satu transaksi

Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk mengecek progres copy di **replica**.

```sql
SELECT subscription_name, status FROM pglogical.show_subscription_status();
SELECT sync_kind, sync_nspname, sync_relname, sync_status FROM pglogical.local_sync_status;
SELECT relid::regclass AS table_name, pg_size_pretty(bytes_processed) AS copied, tuples_processed AS rows_copied FROM pg_stat_progress_copy;
```

Hasil yang diterima: copied dan rows_copied terus naik.

Berikut adalah command untuk masuk ke psql di **master**.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql "$MASTER_DSN"
```

Berikut adalah command untuk memantau WAL yang ditahan slot di master selama copy.

```sql
SELECT slot_name, active, wal_status, pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), restart_lsn)) AS wal_ditahan FROM pg_replication_slots WHERE plugin = 'pglogical_output';
```

Hasil yang diterima: wal_ditahan naik selama copy. Nilai ini dipantau bersama FreeStorageSpace dan TransactionLogsDiskUsage supaya storage RDS tidak penuh.

## 4.12 Cek status subscription

Copy dianggap selesai jika status replicating dan semua sync_status = r. Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk mengecek status subscription.

```sql
SELECT subscription_name, status, provider_node, slot_name FROM pglogical.show_subscription_status();
SELECT sync_kind, sync_nspname, sync_relname, sync_status FROM pglogical.local_sync_status WHERE sync_status <> 'r';
```

Hasil yang diterima: status replicating dan query kedua tidak menampilkan baris apa pun.

Catatan: jika status down, maka cek journalctl -u postgresql-18 dan log di \$PGDATA/log.

## 4.13 Pasang sisa index

Setelah status replicating, sisa index dan unique constraint dari post_\$SOURCE_ID.sql dipasang satu per satu, di luar transaksi, sebagai CREATE INDEX CONCURRENTLY. pg_dump menulis CREATE INDEX biasa, jadi perlu diubah dulu. Berikut adalah command untuk membuat daftar index lalu menjalankannya.

```bash
source /root/replikasi/<SOURCE_ID>.env
grep -E '^CREATE (UNIQUE )?INDEX ' /var/lib/pgsql/post_$SOURCE_ID.sql \
  | sed -E 's/^CREATE (UNIQUE )?INDEX /CREATE \1INDEX CONCURRENTLY /' > /var/lib/pgsql/idx_$SOURCE_ID.sql
vi /var/lib/pgsql/idx_$SOURCE_ID.sql
```

Di vi, hapus index yang sudah dipasang bersama primary key (index replica identity). File dijalankan tanpa BEGIN, satu statement per baris.

Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -E -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk menjalankan file index lalu mengecek index yang INVALID.

```sql
\i /var/lib/pgsql/idx_<SOURCE_ID>.sql
SELECT indexrelid::regclass FROM pg_index WHERE NOT indisvalid;
```

Hasil yang diterima: query terakhir tidak menampilkan index apa pun.

Catatan: jika CREATE INDEX CONCURRENTLY gagal, maka index tertinggal INVALID. Jalankan DROP INDEX CONCURRENTLY untuk index tersebut, lalu ulangi. Unique constraint (ADD CONSTRAINT ... UNIQUE) dibuat dulu sebagai CREATE UNIQUE INDEX CONCURRENTLY, lalu ALTER TABLE ... ADD CONSTRAINT ... UNIQUE USING INDEX. Statement lain di post-data (foreign key, trigger): \<ISI: dipasang di replica atau tidak>.

## 4.14 Clean up, rotate credentials

Setelah replikasi berjalan (status replicating dan semua index terpasang), rekomendasi clean up diantaranya:

- Hapus file kerja di /var/lib/pgsql (pre_\<SOURCE_ID>.sql, post_\<SOURCE_ID>.sql, pk_\<SOURCE_ID>.sql, idx_\<SOURCE_ID>.sql) atau pindahkan ke lokasi arsip.
- Ganti (rotate) password user replication_backup di master, lalu update /var/lib/pgsql/.pgpass di replica. Subscription tetap berjalan karena koneksi baru membaca .pgpass.
- Pastikan password tidak tersimpan di file env, history shell, atau DSN subscription.
- Keluar dari sesi tmux replikasi jika sudah tidak dipakai.
