---
title: "Tahap Instalasi"
project: Imigrasi/Piranti
tags: [replication, postgres, pglogical]
source: "https://docs.google.com/document/d/1gQJbl6Z1EwsavDAqmKDYl4HIlifHwVF04zkwu_8Q67c/edit"
---

# 2 Tahap Instalasi

Semua perintah di tahap ini dijalankan di VM replica sebagai root.

## 2.1 Masuk ke VM replica

Gunakan tmux supaya proses yang lama (dump, restore, sinkronisasi awal) tidak putus jika koneksi SSH terputus.

```bash
tmux new -s replikasi
sudo -i
```

Jika koneksi terputus, masuk lagi ke VM lalu sambungkan kembali ke sesi yang sama:

```bash
tmux attach -t replikasi
```

Catatan: jika tmux belum terpasang, pasang dengan `sudo dnf install -y tmux`. Keluar dari sesi tanpa menghentikan proses dengan `Ctrl+b` lalu `d`.

Catatan: jika sudo ditolak, minta akses sudoers untuk user sendiri ke admin VM. user postgres tidak memerlukan sudoers

## 2.2 Cek kondisi VM

Beberapa hal yang dicek diantaranya:

- OS AlmaLinux 9 dan arsitektur x86_64
- /data/postgresql dan /data/pg_wal sudah mount di partisi yang berbeda
- NTPSynchronized=yes
- SELinux Enforcing (jangan dimatikan)
- Belum ada paket postgresql-server terpasang

```bash
grep -E '^(NAME|VERSION_ID)=' /etc/os-release
uname -m
findmnt -no SOURCE,TARGET --mountpoint /data/postgresql
findmnt -no SOURCE,TARGET --mountpoint /data/pg_wal
df -h /data/postgresql /data/pg_wal
timedatectl show -p NTPSynchronized
getenforce
systemctl is-active firewalld
rpm -qa 'postgresql*'
```

Catatan: jika findmnt tidak keluar apa-apa atau SOURCE kedua mount sama, maka perbaiki partisi VM dulu. Kalau rpm hanya menampilkan postgresql-libs bawaan OS maka bisa dilanjutkan, sedangkan jika ada postgresql-server maka berhenti dan cek dulu.

## 2.3 Simpan variabel di env file

Variable yang dibutuhkan untuk proses ini disimpan di file /root/replikasi/\<SOURCE_ID>.env supaya nilai variable tidak hilang ketika SSH terputus. Berikut ada command untuk mengenerate file environment ganti semua \<ISI: ...> dengan value yang sesuai.

Supaya nilai tidak hilang kalau SSH putus, variabel disimpan di file. Ganti semua \<ISI: ...> sebelum lanjut.

```bash
mkdir -p /root/replikasi
cat > /root/replikasi/<SOURCE_ID>.env <<'EOF'
# PostgreSQL 18 di AlmaLinux 9
export PGBIN=/usr/pgsql-18/bin
export PGDATA_LINK=/var/lib/pgsql/18/data
export PGDATA=/data/postgresql/18/main
export PGWAL=/data/pg_wal/18/main
export PG_UNIT=postgresql-18
# master di RDS/Aurora (selalu writer endpoint)
export SOURCE_ID=<ISI: Nama identifier database - hanya lowercase char dan angka>
export SOURCE_HOST=<ISI: AWS hostname atau domain>
export SOURCE_PORT=5432
export SOURCE_DB=<ISI: Isi Nama database di dalam Postgres AWS RDS/Aurora>
export TARGET_DB=<ISI: Isi Nama database di tujuan replika, sebaiknya disamakan SOURCE_DB>
export REPL_USER=replication_backup
export REPL_SCHEMAS="'<ISI: schema yang direplikasi, pisahkan dengan koma>'"
export MASTER_DSN="host=$SOURCE_HOST port=$SOURCE_PORT dbname=$SOURCE_DB user=$REPL_USER sslmode=require"
# nama node pglogical
export PROVIDER_NODE=provider_${SOURCE_ID}
export SUBSCRIBER_NODE=subscriber_${SOURCE_ID}
export SUBSCRIPTION=sub_${SOURCE_ID}
EOF
chmod 600 /root/replikasi/<SOURCE_ID>.env
source /root/replikasi/<SOURCE_ID>.env
grep -n 'ISI:' /root/replikasi/<SOURCE_ID>.env
```

Hasil grep harus kosong. \<SOURCE_ID> di nama file diganti dengan value SOURCE_ID yang sama (lihat tabel nilai). REPL_SCHEMAS ditulis dengan kutip satu per schema, contoh format: "'schema_a','schema_b'". Kalau SSH putus, cukup sudo -i lalu source /root/replikasi/\<SOURCE_ID>.env lagi.

## 2.4 Cek koneksi ke master

Berikut adalah command untuk mengecek koneksi ke Master Database.

```bash
getent hosts $SOURCE_HOST
timeout 5 bash -c "</dev/tcp/$SOURCE_HOST/$SOURCE_PORT" && echo "port open" || echo "port closed"
```

Hasil yang diterima: getent menampilkan IP endpoint (biasanya IP private lewat VPN/Direct          Connect) dan tercetak port open. Kalau port closed, cek jalur VPN/Direct Connect dan security group RDS untuk 10.0.70.0/25.

## 2.5 Pasang repo PGDG dan paket

Postgres yang kita gunakan adalah postgres dari repo PGDG, bukan dari AppStream AlmaLinux. Berikut step untuk install postgres18 di Alma Linux.

### 2.5.1 PGDG repository

```bash
dnf -qy module disable postgresql
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

### 2.5.2 Server packages

```bash
dnf install -y postgresql18-server postgresql18-contrib pglogical_18-2.4.6 pgaudit_18 policycoreutils-python-utils
```

### 2.5.3 pglogical, pinned version

```bash
rpm -q postgresql18-server pglogical_18 pgaudit_18
```

Catatan: rpm -q harus menampilkan postgresql18-server, pglogical_18-2.4.6 dan pgaudit_18. Extension pgcrypto dan uuid-ossp sudah ada di postgresql18-contrib. Kalau VM tidak bisa akses internet, pakai mirror atau proxy: \<ISI: mirror/proxy repo internal, kalau ada>.

## 2.6 Simpan password di .pgpass

Password replication_backup tidak perlu ditulis di dokumen atau di command, cukup di file .pgpass milik user postgres.

```bash
sudo -u postgres touch /var/lib/pgsql/.pgpass
chmod 600 /var/lib/pgsql/.pgpass
echo "$SOURCE_HOST:$SOURCE_PORT:*:$REPL_USER:GANTI_PASSWORD" >> /var/lib/pgsql/.pgpass
vi /var/lib/pgsql/.pgpass
```

Di vi ganti GANTI_PASSWORD dengan \<ISI: password replication_backup>. Kalau password mengandung : atau \\ tambahkan \\ di depannya.

## 2.7 Tes login ke master

Berikut adalah command untuk tes login ke Master Database memakai user replication_backup dan password dari file .pgpass.

```bash
source /root/replikasi/<SOURCE_ID>.env
sudo -u postgres $PGBIN/psql "$MASTER_DSN" \
  -c "select version();" \
  -c "show wal_level;" \
  -c "show rds.logical_replication;" \
  -c "show shared_preload_libraries;" \
  -c "show output_plugin_libraries;"
```

Hasil yang diterima diantaranya:

- wal_level = logical
- rds.logical_replication = on
- shared_preload_libraries memuat pglogical
- output_plugin_libraries memuat pglogical_output (PostgreSQL 18.6)

Catatan: jika login gagal karena password, maka cek lagi isi file /var/lib/pgsql/.pgpass dan pastikan permission-nya 600.
