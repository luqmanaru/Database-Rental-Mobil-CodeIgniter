# Database-Rental-Mobil-CodeIgniter

![MySQL](https://img.shields.io/badge/MySQL-5.x-green.svg)
![PHP](https://img.shields.io/badge/PHP-7.x-blue.svg)
![CodeIgniter](https://img.shields.io/badge/CodeIgniter-3.x-orange.svg)

Struktur database lengkap untuk aplikasi rental mobil dengan tabel admin, customer, mobil, dan transaksi yang siap digunakan dengan framework CodeIgniter.

## Struktur Database

### Tabel: admin

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id_admin | INT(11) | Primary Key, Auto Increment |
| username | VARCHAR(50) | Username untuk login |
| password | VARCHAR(255) | Password terenkripsi |
| nama_admin | VARCHAR(100) | Nama lengkap admin |
| email | VARCHAR(100) | Email admin |
| level | ENUM('admin', 'superadmin') | Level akses admin |
| status | ENUM('aktif', 'nonaktif') | Status akun admin |
| created_at | TIMESTAMP | Waktu pembuatan record |

### Tabel: customer

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id_customer | INT(11) | Primary Key, Auto Increment |
| nama_customer | VARCHAR(100) | Nama lengkap customer |
| alamat | TEXT | Alamat customer |
| no_hp | VARCHAR(15) | Nomor HP customer |
| no_ktp | VARCHAR(20) | Nomor KTP customer |
| email | VARCHAR(100) | Email customer |
| created_at | TIMESTAMP | Waktu pembuatan record |

### Tabel: mobil

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id_mobil | INT(11) | Primary Key, Auto Increment |
| nama_mobil | VARCHAR(100) | Nama model mobil |
| merk | VARCHAR(50) | Merek mobil |
| jenis | VARCHAR(50) | Jenis mobil (MPV, SUV, dll) |
| plat_nomor | VARCHAR(15) | Plat nomor mobil |
| tahun | INT(4) | Tahun pembuatan mobil |
| harga_sewa | DECIMAL(10,2) | Harga sewa per hari |
| status | ENUM('tersedia', 'disewa', 'perbaikan') | Status ketersediaan mobil |
| created_at | TIMESTAMP | Waktu pembuatan record |

### Tabel: transaksi

| Kolom | Tipe | Keterangan |
|-------|------|------------|
| id_transaksi | INT(11) | Primary Key, Auto Increment |
| id_customer | INT(11) | Foreign Key ke customer.id_customer |
| id_mobil | INT(11) | Foreign Key ke mobil.id_mobil |
| id_admin | INT(11) | Foreign Key ke admin.id_admin |
| tanggal_sewa | DATE | Tanggal mulai sewa |
| tanggal_kembali | DATE | Tanggal selesai sewa |
| total_bayar | DECIMAL(10,2) | Total pembayaran |
| status | ENUM('proses', 'selesai', 'batal') | Status transaksi |
| created_at | TIMESTAMP | Waktu pembuatan record |

## Relasi Antar Tabel
<img width="267" height="465" alt="image" src="https://github.com/user-attachments/assets/5afb997a-8448-41e3-b94b-a3e8d5a91deb" />

## ERD (Deskripsi)
-- Deskripsi relasi antar tabel
-- Tabel admin: Menyimpan data administrator sistem
-- Tabel customer: Menyimpan data pelanggan rental
-- Tabel mobil: Menyimpan data kendaraan yang disewakan
-- Tabel transaksi: Menyimpan data transaksi penyewaan mobil
-- 
-- Relasi:
-- - transaksi.id_customer -> customer.id_customer (Many-to-One)
-- - transaksi.id_mobil -> mobil.id_mobil (Many-to-One)
-- - transaksi.id_admin -> admin.id_admin (Many-to-One)

## Query Dasar
- Menampilkan semua mobil yang tersedia
``SELECT * FROM mobil WHERE status = 'tersedia';``

- Menampilkan transaksi dengan detail customer dan mobil
``SELECT t.id_transaksi, c.nama_customer, m.nama_mobil, m.plat_nomor, t.tanggal_sewa, t.tanggal_kembali, t.total_bayar, t.status
FROM transaksi t
JOIN customer c ON t.id_customer = c.id_customer
JOIN mobil m ON t.id_mobil = m.id_mobil;``

- Menampilkan laporan pendapatan per bulan
``SELECT MONTH(tanggal_sewa) AS bulan, YEAR(tanggal_sewa) AS tahun, SUM(total_bayar) AS total_pendapatan
FROM transaksi
WHERE status = 'selesai'
GROUP BY MONTH(tanggal_sewa), YEAR(tanggal_sewa)
ORDER BY tahun, bulan;``

---
**luqmanaru**

Ini adalah proyek pengembangan web lanjut untuk memenuhi tugas kuliah Pemrograman Web Lanjut
