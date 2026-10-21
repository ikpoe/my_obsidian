---
title: "Tahap Verifikasi"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# Tahap Verifikasi

Verifikasi ini baru dipakai setelah status replicating dan semua sync_status = r. Beberapa hal yang dicek diantaranya:

- Slot pglogical di master active = t dan lag kecil
- Jumlah baris beberapa tabel sama antara master dan replica
- Nilai sequence sebelum replica dipakai untuk menulis

## Cek slot di master

Berikut adalah command untuk mengecek slot pglogical di master.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN" -c "SELECT slot_name, active, wal_status, pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), confirmed_flush_lsn)) AS lag FROM pg_replication_slots WHERE plugin = 'pglogical_output';"
```

Hasil yang diterima: active = t, wal_status reserved atau extended, dan lag dalam hitungan KB/MB.

## Bandingkan jumlah baris

Berikut adalah command untuk membandingkan jumlah baris satu tabel di master dan di replica. Ganti \<ISI: ...> dengan nama tabel yang akan dicek.

```bash
source /root/replikasi/<SOURCE_ID>.env
export CEK_TABEL=<ISI: schema.tabel untuk dicek>
sudo -u postgres $PGBIN/psql "$MASTER_DSN" -Atc "SELECT count(*) FROM $CEK_TABEL;"
sudo -u postgres $PGBIN/psql -d $TARGET_DB -Atc "SELECT count(*) FROM $CEK_TABEL;"
```

Catatan: untuk tabel yang sering diisi, jumlahnya bisa berbeda sedikit karena lag. Jika diulang beberapa kali, maka selisihnya harus semakin kecil.

## Sinkron sequence

Mendaftarkan sequence di master tidak menyalin nilai sequence. Nilai sequence di replica adalah nilai saat dump. Sync manual hanya perlu sebelum replica dipakai untuk menulis (mis. promote). pglogical.synchronize_sequence dijalankan di replica, bukan ke \$MASTER_DSN. Berikut adalah command untuk sync sequence.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql -d $TARGET_DB -c "SELECT pglogical.synchronize_sequence(c.oid) FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace WHERE c.relkind = 'S' AND n.nspname IN ($REPL_SCHEMAS);"
```
