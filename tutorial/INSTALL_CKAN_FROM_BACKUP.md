# Tutorial Instalasi CKAN dari Backup dan Penambahan Fitur
## Pendahuluan
Dokumen ini menjelaskan langkah lengkap untuk:
- Menyiapkan environment CKAN di localhost
- Menginstal CKAN dari backup existing system
- Melakukan restore database
- Menjalankan CKAN hingga siap digunakan
- Menambahkan fitur kustom tanpa plugin (modifikasi core)
## Requirement Sistem
### Sistem Operasi
Linux: Ubuntu 22.04.1 LTS (WAJIB)

Disarankan menggunakan Ubuntu versi ini untuk menghindari masalah kompatibilitas library.
### Versi Software
Versi Komponen:
- CKAN	2.10.1
- Python	3.10.6
- PostgreSQL	14.18
- Apache Solr	8.x
### File Backup yang Diperlukan
File backup diperoleh dari DISKOMINFO dan WAJIB lengkap.
- File Folder usr:         Ini adalah virtualenv CKAN dari backup.
- File Folder etc:         Berisi ckan.ini, konfigurasi Solr, dan config tambahan.
- File Folder var:         Berisi data CKAN: uploaded files, attachments, datastore, private files.
- File db_ckan_220825.sql: Database Restore SQL dari backup ke PostgreSQL 14.18.
## Konsep Instalasi dari Backup
Metode ini TIDAK melakukan install CKAN dari awal menggunakan pip karena:
- Menghindari perbedaan versi dependency
- Menghindari error incompatibility
- Menjamin sistem sama persis dengan server asal

## Persiapan Awal Server
### Update Sistem
sudo apt update && sudo apt upgrade -y

### Install Dependency Dasar
sudo apt install -y git curl wget build-essential python3.10 python3.10-venv python3-pip libpq-dev openjdk-11-jdk
## Instalasi PostgreSQL 14
### Install PostgreSQL
sudo apt install -y postgresql-14 postgresql-client-14
### Pastikan Service Aktif
sudo systemctl start postgresql\
sudo systemctl enable postgresql
### Buat User dan Database CKAN
sudo -u postgres psql

CREATE USER ckanuser WITH PASSWORD 'passwordkuat';\
CREATE DATABASE ckan_default OWNER ckanuser;\
ALTER USER ckanuser CREATEDB;\
\q

## Restore Database CKAN
### Copy File SQL
cp db_ckan_220825.sql ~/
### Restore Database
sudo -u postgres psql ckan_default < ~/db_ckan_220825.sql
### Verifikasi
sudo -u postgres psql ckan_default\
\dt\
\q

Jika tabel CKAN muncul, restore berhasil.
## Restore Folder Backup
### Restore Virtualenv CKAN
sudo cp -r usr/lib/ckan/default /usr/lib/ckan/\
sudo chown -R www-data:www-data /usr/lib/ckan
### Restore Konfigurasi
sudo cp -r etc/ckan /etc/\
sudo chown -R www-data:www-data /etc/ckan
### Restore Data CKAN
sudo cp -r var/lib/ckan /var/lib/\
sudo chown -R www-data:www-data /var/lib/ckan
## Instalasi dan Konfigurasi Solr
### Install Solr 8
sudo apt install -y solr-tomcat
### Copy Core Solr dari Backup
sudo cp -r /etc/ckan/solr /var/lib/solr/\
sudo chown -R solr:solr /var/lib/solr
### Restart Solr
sudo systemctl restart solr
### Cek Solr
http://localhost:8983/solr
## Konfigurasi CKAN
### Edit ckan.ini
sudo nano /etc/ckan/ckan.ini

Pastikan:
- sqlalchemy.url sesuai database
- ckan.site_url sesuai localhost
- ckan.storage_path mengarah ke /var/lib/ckan

## Menjalankan CKAN
### Aktifkan Virtualenv
source /usr/lib/ckan/default/bin/activate
### Jalankan CKAN
cd /usr/lib/ckan/default/src/ckan\
paster serve /etc/ckan/ckan.ini
### Akses
http://localhost:5000

Jika halaman utama muncul, CKAN berhasil dijalankan.
## Penambahan Fitur Kustom (Tanpa Plugin)
Silahkan menuju ke ckan-opendata-customization/docs/ untuk informasi lebih lanjut
## Penutup
Tutorial ini dibuat secara general dan mungkin terdapat beberapa perbedaan.

Jangan malu bertanya jika terdapat kesulitan.

"Malu bertanya sesat di jalan, Kalau ga mau nanya siap siap kebingungan"
## Informasi Tambahan
Berikut merupakan link dari percakapan saya bersama yang mulia "chat GPT" seputar instalasi dan migrasi CKAN yang diharapkan bisa membantu.

https://chatgpt.com/share/696098de-893c-800b-a11b-3d83d211c819

"maaf jika terdapat kata kata yang kurang enak dan ambigu pada percakapan saya dengan chat GPT ini"
