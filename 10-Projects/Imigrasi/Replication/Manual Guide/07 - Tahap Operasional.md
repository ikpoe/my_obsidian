---
title: "Tahap Operasional"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 7 Tahap Operasional

## 7.1 Tabel tanpa identity

Beberapa hal yang perlu diperhatikan untuk tabel tanpa identity diantaranya:

- Cek ulang kelayakan setiap ada tabel baru (query di Tahap Replikasi Database)
- Tabel tanpa identity hanya boleh masuk set default_insert_only, dan hanya jika aplikasinya tidak pernah UPDATE atau DELETE
- Selain itu tabel tidak didaftarkan

## 7.2 Slot replikasi, max_slot_wal_keep_size dan alarm

Slot pglogical di master menahan WAL selama replica belum menerima data. Jika replica mati lama, maka storage RDS bisa penuh dan master ikut berhenti. Pengamannya diantaranya:

- Setting max_slot_wal_keep_size di parameter group master: \<ISI: batas WAL dalam MB>
- Jika batas terlewati, maka slot menjadi lost dan replica harus sync ulang dari awal
- CloudWatch alarm OldestReplicationSlotLag: \<ISI: threshold>
- CloudWatch alarm TransactionLogsDiskUsage dan FreeStorageSpace: \<ISI: threshold>
- Alarm dikirim ke \<ISI: SNS topic / email / channel>

## 7.3 Failover dan upgrade master

Beberapa hal yang perlu dicek saat failover atau upgrade master diantaranya:

- Slot logical bisa hilang saat failover Multi-AZ/Aurora atau major upgrade master \<ISI: cek dokumentasi AWS sesuai versi engine>
- Setelah failover atau upgrade, cek slot di master (query di Tahap Verifikasi)
- Jika slot hilang, maka replica di-sync ulang (lihat bagian berikut)
- Selalu pakai writer endpoint, bukan reader endpoint

## 7.4 Replica mati lama

Langkah yang dilakukan tergantung kondisi slot di master, diantaranya:

- Slot masih ada dan wal_status bukan lost: start PostgreSQL lagi, replica akan mengejar sendiri
- Mati terencana lebih lama dari batas max_slot_wal_keep_size: drop subscription dulu supaya WAL di master tidak tertahan
- Slot dengan wal_status = lost atau slot hilang: sync ulang dari awal dengan command di bawah

Berikut adalah command untuk sync ulang dari awal. Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk drop subscription di replica.

```sql
SELECT pglogical.drop_subscription('<SUBSCRIPTION>');
\q
```

Berikut adalah command untuk masuk ke psql di master.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN"
```

Berikut adalah command untuk mengecek slot di master.

```sql
SELECT slot_name, active, wal_status FROM pg_replication_slots WHERE plugin = 'pglogical_output';
```

Catatan: jika slot pgl_... masih ada dan active = f, maka drop slot dengan command berikut di sesi psql master yang sama.

```sql
SELECT pg_drop_replication_slot('<ISI: slot_name pgl_...>');
\q
```

Berikut adalah command untuk menghapus database tujuan di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/dropdb $TARGET_DB
```

Setelah itu ulangi Tahap Replikasi Database mulai dari Buat database tujuan (libc en_US.UTF-8), dengan pemisahan pre-data dan post-data. Provider node yang sudah ada di master tidak dibuat ulang.

## 7.5 Menambahkan tabel baru ke replikasi

Tabel baru di master tidak otomatis ikut direplikasi, walaupun strukturnya sudah tercipta di replica lewat Replikasi DDL. Tabel harus didaftarkan ke replication set satu per satu. Langkah yang dilakukan diantaranya:

- Cek kelayakan tabel di master (punya primary key atau replica identity)
- Pastikan struktur tabel sudah ada di replica dan tabelnya masih kosong
- Daftarkan tabel ke replication set dengan synchronize_data := true, supaya data yang sudah ada ikut disalin
- Cek status tabel di replica sampai replicating

Ganti \<NEW_TABLE> dengan nama tabel baru (schema.tabel). Berikut adalah command untuk masuk ke psql di master.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN"
```

Berikut adalah command untuk cek kelayakan tabel di master.

```sql
SELECT c.oid::regclass AS tabel, c.relreplident,
       EXISTS (SELECT 1 FROM pg_constraint k WHERE k.conrelid = c.oid AND k.contype = 'p') AS punya_pk
FROM pg_class c WHERE c.oid = '<NEW_TABLE>'::regclass;
\q
```

Hasil yang diterima: punya_pk = t, atau relreplident = f / i. Jika tidak punya identity, maka ikuti aturan di 7.1.

Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk cek struktur tabel di replica.

```sql
SELECT count(*) FROM <NEW_TABLE>;
\q
```

Hasil yang diterima: count = 0. Jika tabel belum ada di replica (Replikasi DDL belum terpasang), maka salin strukturnya dulu dari master dengan command berikut.

```bash
source /root/replikasi/<SOURCE_ID>.env
export NEW_TABLE='<ISI: schema.tabel_baru>'
sudo -u postgres $PGBIN/pg_dump "$MASTER_DSN" --schema-only --no-owner --no-privileges -t "$NEW_TABLE" \
  | sudo -u postgres $PGBIN/psql -d $TARGET_DB -v ON_ERROR_STOP=1
```

Berikut adalah command untuk masuk ke psql di master.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN"
```

Berikut adalah command untuk mendaftarkan tabel ke replication set.

```sql
SET lock_timeout = '3s';
SELECT pglogical.replication_set_add_table('default', '<NEW_TABLE>'::regclass, synchronize_data := true);
\q
```

Hasil yang diterima: t. Untuk tabel insert-only tanpa identity, ganti 'default' dengan 'default_insert_only'.

Berikut adalah command untuk masuk ke psql di replica.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command untuk cek status tabel di replica.

```sql
SELECT * FROM pglogical.show_subscription_table('<SUBSCRIPTION>', '<NEW_TABLE>'::regclass);
```

Hasil yang diterima: status berubah dari synchronizing menjadi replicating. Tabel besar butuh waktu lebih lama, pantau dengan query yang sama.

Catatan: jika gagal karena lock timeout, maka jalankan lagi di jam sepi. Jika status tetap down atau sync gagal, maka cek log di \$PGDATA/log, kosongkan tabel di replica, lalu ulangi dengan pglogical.alter_subscription_resynchronize_table('<SUBSCRIPTION>', '<NEW_TABLE>'::regclass) di sesi psql replica.

## 7.6 Kolom atau tabel baru di master yang belum ada di replica

DDL yang dijalankan langsung di master (tanpa pglogical.replicate_ddl_command) tidak ikut ke replica. Gejalanya subscription menjadi down dan apply worker mencoba ulang setiap 3 menit dengan error seperti unknown column name <KOLOM>. Log PostgreSQL berformat JSON, jadi dibaca dengan jq. Berikut adalah command untuk melihat error pglogical di replica.

```bash
sudo jq -r 'select(.error_severity|test("ERROR|FATAL|PANIC")) | [.timestamp, .pid, .error_severity, .backend_type, .message] | @tsv' /var/lib/pgsql/18/data/log/postgresql-$(date +%a).json | tail -50
```

Berikut adalah command untuk melihat definisi kolom di master (sesi psql master, DB source).

```sql
SELECT table_schema, table_name, data_type, character_maximum_length, numeric_precision, numeric_scale, is_nullable, column_default
FROM information_schema.columns
WHERE column_name = '<KOLOM>';
```

Jalankan query yang sama di sesi psql replica. Tabel yang muncul di master tetapi tidak muncul di replica adalah tabel yang perlu ditambah kolomnya. Berikut adalah command untuk menambahkan kolom di replica, dengan tipe, NOT NULL dan DEFAULT persis sama dengan master.

```sql
BEGIN;
ALTER TABLE <schema>.<table> ADD COLUMN <KOLOM> <tipe> [NOT NULL] [DEFAULT <nilai>];
COMMIT;
```

Berikut adalah SQL untuk checking.

```sql
SELECT table_name, data_type, is_nullable, column_default
FROM information_schema.columns
WHERE column_name = '<KOLOM>' ORDER BY table_name;
SELECT subscription_name, status FROM pglogical.show_subscription_status();
```

Hasil yang diterima: definisi kolom sama dengan master, dan dalam sekitar 3 menit status kembali replicating tanpa disable/enable subscription.

Catatan: jika kolom di master punya DEFAULT, maka DEFAULT wajib ikut di replica, karena baris lama di master sudah terisi nilai default dan tidak dikirim ulang. Jika error berganti ke kolom atau tabel lain, maka ulangi langkah ini untuk DDL berikutnya. Waspadai tanda kutip lengkung saat copy-paste DDL.

## 7.7 Membuat user read-only untuk pemohon akses

Permintaan akses ke replica diberi user read-only, karena tabel replica diisi oleh pglogical dan tidak boleh ditulis. Semua perintah dijalankan di VM replica. Data yang dibutuhkan dari pemohon: nama user, database, schema yang diakses, dan range IP (CIDR). Berikut adalah command untuk generate password.

```bash
openssl rand -base64 24 | tr -d '/+=' | cut -c1-24
```

Simpan password di password manager, jangan ditempel di chat atau tiket. Berikut adalah command untuk masuk ke psql sebagai superuser di replica.

```bash
sudo -u postgres psql -d <TARGET_DB>
```

Hasil yang diterima: prompt <TARGET_DB>=#. Jika prompt <TARGET_DB>=>, maka sesi tidak superuser atau tersambung ke master, CREATE ROLE akan gagal dengan permission denied.

Berikut adalah command untuk membuat user dan memberi akses SELECT ke semua schema aplikasi.

```sql
CREATE ROLE <APP_USER> LOGIN PASSWORD '<PASSWORD>' CONNECTION LIMIT 20;
GRANT CONNECT ON DATABASE <TARGET_DB> TO <APP_USER>;
ALTER ROLE <APP_USER> SET default_transaction_read_only = on;

SELECT format('GRANT USAGE ON SCHEMA %I TO <APP_USER>; GRANT SELECT ON ALL TABLES IN SCHEMA %I TO <APP_USER>; ALTER DEFAULT PRIVILEGES IN SCHEMA %I GRANT SELECT ON TABLES TO <APP_USER>;', nspname, nspname, nspname)
FROM pg_namespace
WHERE nspname NOT IN ('pg_catalog','information_schema','pglogical','pg_toast')
  AND nspname NOT LIKE 'pg_temp%' AND nspname NOT LIKE 'pg_toast_temp%'
\gexec
```

Catatan: jika pemohon hanya butuh schema tertentu, maka tambahkan AND nspname IN ('<schema>') pada query. ALTER DEFAULT PRIVILEGES hanya berlaku untuk tabel yang dibuat postgres; jika tabel baru dibuat role lain, maka tambahkan FOR ROLE <owner>.

Berikut adalah SQL untuk checking.

```sql
\du <APP_USER>
SELECT table_schema, count(*) FROM information_schema.role_table_grants
WHERE grantee = '<APP_USER>' AND privilege_type = 'SELECT'
GROUP BY 1 ORDER BY 1;
SHOW ssl;
\q
```

Hasil yang diterima: jumlah tabel per schema sama dengan report di 4.3, schema pglogical tidak muncul. Catat hasil SHOW ssl untuk pg_hba.

Berikut adalah command untuk menambahkan pg_hba. Pakai host jika SHOW ssl off, hostssl jika on.

```bash
echo "host  <TARGET_DB>  <APP_USER>  <CIDR>  scram-sha-256" >> /var/lib/pgsql/18/data/pg_hba.conf
sudo -u postgres psql -c "SELECT pg_reload_conf();"
sudo -u postgres psql -c "SELECT line_number, type, database, user_name, address, netmask, error FROM pg_hba_file_rules WHERE '<APP_USER>' = ANY(user_name);"
```

Hasil yang diterima: 1 baris, kolom error kosong.

Catatan: jika error hostssl record cannot match because SSL is disabled, maka ganti hostssl menjadi host di baris tersebut lalu reload.

Berikut adalah command untuk membuka firewall.

```bash
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="<CIDR>" port port="5432" protocol="tcp" accept'
firewall-cmd --reload
firewall-cmd --list-rich-rules
```

Informasi untuk pemohon: Host (IP VM replica, hostname -I), Port 5432, Database <TARGET_DB>, Username <APP_USER>, password lewat jalur aman, SSL Mode disable jika ssl off atau require jika ssl on.
