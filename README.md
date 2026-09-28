# Tugas-praktikum-3-
# Tugas Akhir Pemrograman Web: Product Manager

Aplikasi web manajemen produk berbasis **PHP (Native)**, **MySQL (PDO)**, dan **CSS UI Styling (Flexbox & Box Model)**. Projek ini dibuat berdasarkan kriteria modul Pertemuan 3 Pemrograman Web.

---

##  Struktur Direktori Projek

```text
product-manager/
├── config/
│   └── db.php           # Koneksi PDO MySQL & Inisialisasi Token CSRF Session
├── database/
│   └── store_db.sql     # Skema Database & Seed Data Awal
├── public/
│   ├── assets/
│   │   └── style.css    # Styling UI (Box Model, Flexbox Card Grid, Form, Badge)
│   ├── create.php       # Form & Process CREATE (Post-Redirect-Get)
│   ├── delete.php       # Handler DELETE (POST + CSRF Protection)
│   ├── edit.php         # Form & Process UPDATE
│   └── index.php        # Halaman Utama READ (+ Search/Filter GET)
└── README.md            # Dokumentasi & Panduan Penggunaan
```

---

##  Fitur Utama & Kepatuhan Modul

1. **CRUD Lengkap & PDO Prepared Statement**:
   - **Create**: Tambah produk baru dengan validasi server-side.
   - **Read**: Tampilan card responsif menggunakan Flexbox.
   - **Update**: Edit data produk berdasarkan ID.
   - **Delete**: Hapus produk menggunakan method POST dan konfirmasi antarmuka.
   - Menggunakan `PDO::prepare()` dan `execute()` untuk mencegah **SQL Injection**.

2. **Validasi & Proteksi Keamanan**:
   - **Server-Side Validation**:
     - `name`: Minimal 3 karakter, wajib diisi, dan harus unik.
     - `price`: Harus berupa angka (float) `> 0`.
     - `stock`: Harus berupa angka bulat (int) `>= 0`.
   - **Pencegahan XSS**: Menggunakan `htmlspecialchars($val, ENT_QUOTES, 'UTF-8')` pada seluruh output HTML.
   - **Proteksi CSRF**: Menggunakan token acak pada session (`hash_equals`) untuk aksi `delete.php`.
   - **Pola PRG (Post-Redirect-Get)**: Mencegah pengiriman data ganda saat pengguna melakukan *refresh* halaman setelah operasi `INSERT` / `UPDATE` / `DELETE`.

3. **UI & Layout Responsif**:
   - **CSS Box Model**: Pengaturan `box-sizing: border-box`, margin, padding, dan border yang presisi.
   - **Flexbox Grid**: Card produk menyesuaikan ukuran layar secara responsif (`flex-wrap`, `gap`, `flex: 1 1 300px`).

4. **Fitur Bonus**:
   - **Search / Filter GET**: Pencarian produk berdasarkan nama atau kategori secara aman.

---

##  Cara Menjalankan Projek

### 1. Persiapan Database
1. Buka **phpMyAdmin** atau MySQL CLI (misalnya melalui XAMPP / Laragon).
2. Import file SQL yang berada di `database/store_db.sql`:
   ```sql
   CREATE DATABASE IF NOT EXISTS store_db;
   USE store_db;
   ```
   Atau impor file `database/store_db.sql` secara langsung melalui menu **Import** di phpMyAdmin.

### 2. Menjalankan Server PHP
Anda dapat menggunakan **PHP Built-in Server** atau web server bawaan **XAMPP / Laragon**:

#### Menggunakan PHP CLI:
Buka terminal/Command Prompt di folder `product-manager` dan jalankan:
```bash
php -S localhost:8000 -t public
```
Kemudian buka browser dan kunjungi: `http://localhost:8000`

#### Menggunakan XAMPP / Laragon:
Pindahkan / copy folder `product-manager` ke dalam folder `htdocs` (XAMPP) atau `www` (Laragon), lalu akses:
`http://localhost/product-manager/public/index.php`

---

##  Pengujian Modul & Checklist

| Skenario Pengujian | Hasil yang Diharapkan | Status |
| :--- | :--- | :---: |
| **Tambah Produk Valid** | Data tersimpan, redirect ke `index.php` dengan notifikasi sukses. | ✅ |
| **Nama < 3 Karakter** | Ditolak oleh server dengan pesan error. | ✅ |
| **Harga / Stok Negatif** | Ditolak oleh server dengan pesan error. | ✅ |
| **Refresh Setelah Create** | Tidak ada duplikasi data (terbukti menggunakan Pola PRG). | ✅ |
| **Input Tag HTML (`<b>Promo</b>`)** | Tampil sebagai teks biasa (Terproteksi dari XSS). | ✅ |
| **Hapus Tanpa Token CSRF** | Mengembalikan status `403 Forbidden`. | ✅ |
| **Uji Layar Sempit (Mobile)** | Card produk turun dengan rapi (Flexbox responsive). | ✅ |

---.
