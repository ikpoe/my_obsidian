---
title: "Tahap Konfigurasi"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 3 Tahap Konfigurasi Postgres

Semua perintah di tahap ini dijalankan di VM replica sebagai root, cukup sekali per VM. Jika SSH sempat terputus, maka load lagi env file dengan source /root/replikasi/\<SOURCE_ID>.env.

## 3.1 Siapkan direktori data dan WAL

Sebelum menyiapkan direktori, cek dulu ukuran database di master dan kapasitas partisi data di replica. Data inventaris bisa saja tidak terupdate, jadi ukuran selalu dicek langsung di master. Berikut adalah command untuk cek ukuran semua database di master (dari psql ke master).

```sql
SELECT datname, pg_size_pretty(pg_database_size(datname)) AS size
FROM pg_database
WHERE datname NOT IN ('template0','template1','rdsadmin')
ORDER BY pg_database_size(datname) DESC;
```

Berikut adalah command untuk cek kapasitas partisi di replica.

```bash
df -hT /data/postgresql /data/pg_wal
sudo vgs
```

Hasil yang diterima: Avail di /data/postgresql minimal 1.5x total ukuran database yang akan direplikasi ke VM ini (data, index yang dibangun ulang saat initial copy, dan ruang tumbuh). /data/pg_wal cukup 100G.

Catatan: jika kapasitas kurang, maka minta perbesar disk data terlebih dahulu, jangan lanjut ke initial copy. Jika disk sudah diperbesar di hypervisor, maka jalankan echo 1 > /sys/class/block/sdb/device/rescan, pvresize /dev/sdb, lvextend -l +100%FREE /dev/vg_data_db/lv_postgresql, lalu xfs_growfs /data/postgresql. Initial copy yang gagal karena disk penuh muncul di log sebagai No space left on device, lalu table copy failed, dan subscription menjadi down.

Data dan WAL berada di partisi yang terpisah. PGDATA_LINK /var/lib/pgsql/18/data (path bawaan PGDG) adalah symlink ke \$PGDATA /data/postgresql/18/main, dan WAL ada di \$PGWAL /data/pg_wal/18/main. Service postgresql-18 dipakai apa adanya, tidak ada Environment=PGDATA dan tidak ada data_directory. Mount root /data/postgresql dan /data/pg_wal tetap milik root dengan permission 0755, chmod 700 hanya pada \$PGDATA dan \$PGWAL. Berikut adalah command untuk menyiapkan direktori sebelum initdb.

```bash
source /root/replikasi/<SOURCE_ID>.env
chown root:root /data/postgresql /data/pg_wal
chmod 755 /data/postgresql /data/pg_wal
mkdir -p "$(dirname $PGDATA_LINK)" "$PGDATA" "$PGWAL"
chown postgres:postgres "$PGDATA" "$PGWAL"
chmod 700 "$PGDATA" "$PGWAL"
# jika /var/lib/pgsql/18/data masih direktori kosong:
rmdir "$PGDATA_LINK"
ln -sT "$PGDATA" "$PGDATA_LINK"
semanage fcontext -a -t postgresql_db_t "/data/postgresql(/.*)?"
semanage fcontext -a -t postgresql_db_t "/data/pg_wal(/.*)?"
restorecon -Rv /data/postgresql /data/pg_wal
ls -ld /data/postgresql /data/pg_wal "$PGDATA" "$PGWAL"
ls -ldZ "$PGDATA_LINK"
```

Hasil yang diterima: /data/postgresql dan /data/pg_wal drwxr-xr-x milik root, \$PGDATA dan \$PGWAL drwx------ milik postgres dengan label postgresql_db_t, dan /var/lib/pgsql/18/data -> /data/postgresql/18/main.

Catatan: jika rmdir gagal karena /var/lib/pgsql/18/data tidak kosong, maka cek dulu isinya, jangan dihapus paksa.

## 3.2 Inisialisasi cluster dengan --waldir

initdb dijalankan terhadap \$PGDATA_LINK (path bawaan service postgresql-18) dengan --waldir=\$PGWAL. Locale cluster tidak diatur di initdb. Locale diatur per database saat CREATE DATABASE, disamakan dengan master (libc en_US.UTF-8). Berikut adalah command untuk inisialisasi cluster.

```bash
source /root/replikasi/<SOURCE_ID>.env
PGSETUP_INITDB_OPTIONS="--waldir=$PGWAL" $PGBIN/postgresql-18-setup initdb
ls -l "$PGDATA/PG_VERSION"
ls -ld "$PGDATA_LINK/pg_wal"
```

Hasil yang diterima: \$PGDATA/PG_VERSION ada di disk data (/data/postgresql/18/main/PG_VERSION) dan \$PGDATA_LINK/pg_wal adalah symlink ke /data/pg_wal/18/main.

Catatan: jika initdb gagal dengan pesan could not access directory ".../pg_wal": Permission denied, maka mount root masih 0700. Jalankan chmod 755 /data/postgresql /data/pg_wal (tetap milik root), pastikan \$PGDATA dan \$PGWAL kosong, lalu ulangi initdb.

## 3.3 postgresql.conf, pg_hba.conf

### 3.3.1 Setting postgresql.conf

Setting tambahan untuk replikasi ditaruh di file terpisah conf.d/replikasi.conf supaya mudah dicek. Nilai memory dihitung per VM dari total RAM dengan aturan berikut:

- max_connections = 200
- shared_buffers = 25% RAM
- effective_cache_size = 75% RAM (hanya petunjuk untuk planner, tidak mengalokasikan memory)
- maintenance_work_mem = RAM/16, maksimal 2048MB

Berikut adalah command untuk menghitung nilai tersebut dari RAM VM.

```bash
RAM_MB=$(free -m | awk '/^Mem:/{print $2}')
echo "RAM: ${RAM_MB} MB"
echo "max_connections = 200"
echo "shared_buffers = $((RAM_MB/4))MB"
echo "effective_cache_size = $((RAM_MB*3/4))MB"
M=$((RAM_MB/16)); [ $M -gt 2048 ] && M=2048; echo "maintenance_work_mem = ${M}MB"
```

Hasil yang diterima: nilai untuk keempat parameter. Contoh untuk VM 32 GB: shared_buffers = 8192MB, effective_cache_size = 24576MB, maintenance_work_mem = 2048MB.

Berikut adalah command untuk membuat file tersebut, ganti \<SHARED_BUFFERS>, \<EFFECTIVE_CACHE_SIZE> dan \<MAINTENANCE_WORK_MEM> dengan hasil command di atas.

```bash
source /root/replikasi/<SOURCE_ID>.env
mkdir -p $PGDATA/conf.d
cat > $PGDATA/conf.d/replikasi.conf <<'EOF'
listen_addresses = '*'
wal_level = logical
shared_preload_libraries = 'pglogical,pg_stat_statements,pgaudit'
max_worker_processes = 16
max_replication_slots = 10
max_wal_senders = 10
track_commit_timestamp = on
max_connections = 200
shared_buffers = <SHARED_BUFFERS>
effective_cache_size = <EFFECTIVE_CACHE_SIZE>
maintenance_work_mem = <MAINTENANCE_WORK_MEM>
EOF
echo "include_dir = 'conf.d'" >> $PGDATA/postgresql.conf
chown -R postgres:postgres $PGDATA/conf.d
```

Catatan: pgaudit selalu di-preload di replica, walaupun tidak semua database di master memakai pgaudit. Jika master memakai pgaudit, maka dump berisi CREATE EXTENSION pgaudit dan preload ini wajib ada sebelum restore schema. Jika hasil cek extension di 2.7 menunjukkan extension lain yang butuh preload, maka tambahkan juga di shared_preload_libraries. Perubahan shared_preload_libraries butuh restart, jadi hanya aman dilakukan sebelum ada subscription.

Catatan: max_worker_processes = 16 tetap. Jika akses ingin dibatasi, maka listen_addresses bisa diganti dengan IP VM replica \<ISI: IP VM replica, kalau mau dibatasi>.

### 3.3.2 Akses ke replica (pg_hba dan firewall)

Replica hanya butuh koneksi keluar ke master, sedangkan akses masuk ke replica dibatasi untuk client yang perlu saja. Berikut adalah command untuk menambahkan rule di pg_hba.conf dan firewall.

```bash
echo "host all all <ISI: subnet client yang boleh akses replica> scram-sha-256" >> $PGDATA/pg_hba.conf
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="<ISI: subnet client yang boleh akses replica>" port port="5432" protocol="tcp" accept'
firewall-cmd --reload
firewall-cmd --list-rich-rules
```

Catatan: ganti \<ISI: ...> dengan subnet yang sesuai sebelum command dijalankan, jika tidak maka PostgreSQL akan gagal membaca pg_hba.conf.

## 3.4 Start PostgreSQL

Berikut adalah command untuk menjalankan service PostgreSQL dan mengecek setting yang sudah aktif.

### 3.4.1 Enable and start

```bash
systemctl enable --now postgresql-18
systemctl status postgresql-18 --no-pager
```

Catatan: jika service gagal start, maka cek journalctl -u postgresql-18 dan log di \$PGDATA/log.

### 3.4.2 Verify

Berikut adalah command untuk masuk ke psql di replica.

```bash
sudo -u postgres $PGBIN/psql
```

Berikut adalah command yang dijalankan di dalam sesi psql tersebut.

```sql
show shared_preload_libraries;
show max_worker_processes;
show data_checksums;
\q
```

Hasil yang diterima: shared_preload_libraries pglogical,pg_stat_statements,pgaudit, max_worker_processes 16 dan data_checksums on.
