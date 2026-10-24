---
title: "Tahap Verifikasi"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 5 Tahap Verifikasi

Verifikasi ini baru dipakai setelah status replicating dan semua sync_status = r. Beberapa hal yang dicek diantaranya:

- Slot pglogical di master active = t dan lag kecil
- Jumlah baris beberapa tabel sama antara master dan replica
- Nilai sequence sebelum replica dipakai untuk menulis

## 5.1 Cek slot di master

Berikut adalah command untuk masuk ke psql di master. Sesi ini tetap dibuka untuk 5.2.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN"
```

Berikut adalah command untuk mengecek slot pglogical di master.

```sql
SELECT slot_name, active, wal_status, pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), confirmed_flush_lsn)) AS lag FROM pg_replication_slots WHERE plugin = 'pglogical_output';
```

Hasil yang diterima: active = t, wal_status reserved atau extended, dan lag dalam hitungan KB/MB.

## 5.2 Bandingkan jumlah baris

Berikut adalah command untuk membandingkan jumlah baris satu tabel di master dan di replica. Ganti \<CEK_TABEL> dengan nama tabel yang akan dicek (schema.tabel). Berikut adalah command di sesi psql master yang sama.

```sql
SELECT count(*) FROM <CEK_TABEL>;
\q
```

Berikut adalah command untuk masuk ke psql di replica. Sesi ini tetap dibuka untuk 5.3.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB
```

Berikut adalah command di sesi psql replica tersebut.

```sql
SELECT count(*) FROM <CEK_TABEL>;
```

Catatan: untuk tabel yang sering diisi, jumlahnya bisa berbeda sedikit karena lag. Jika diulang beberapa kali, maka selisihnya harus semakin kecil.

## 5.3 Sinkron sequence

Mendaftarkan sequence di master tidak menyalin nilai sequence. Nilai sequence di replica adalah nilai saat dump. Sync manual hanya perlu sebelum replica dipakai untuk menulis (mis. promote). pglogical.synchronize_sequence dijalankan di replica, bukan ke \$MASTER_DSN. Berikut adalah command untuk sync sequence, di sesi psql replica yang sama. Ganti \<REPL_SCHEMAS> dengan daftar schema yang direplikasi.

```sql
SELECT pglogical.synchronize_sequence(c.oid) FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace WHERE c.relkind = 'S' AND n.nspname IN (<REPL_SCHEMAS>);
\q
```
