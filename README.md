# Inventory Management System (Laravel)

Pertemuan 5 — Laravel Fundamental: migrasi Inventory Management System P4 (PHP native) ke Laravel.
Anggraini Reka Kalsum (250180025) · Sistem Informasi · Universitas Malikussaleh

Alur: **Request → Route → Controller → Model → Database → Response**

## Fitur
- Autentikasi (login/logout) dan role **admin** / **staff** (middleware `role`)
- Master data kategori, produk, supplier — resource controller + Route::resource
- Relasi Eloquent: Category hasMany Product, Product belongsToMany Supplier, StockMovement
- Barang masuk / keluar memakai `DB::transaction` (`App\Services\StockService`)
- Search, filter, sorting (whitelist), pagination dengan `withQueryString()`
- Dashboard laporan agregat
- Blade layout, component (`x-alert`, `x-field-error`, `x-stat-card`, `x-sort-link`), `@csrf`, validasi + pesan error Indonesia

## Business rules P4 yang dipertahankan
SKU unik · harga/quantity tidak negatif · barang keluar tidak melebihi stok · stok tidak negatif ·
produk berhistori tidak boleh dihapus · halaman internal wajib login · staff tidak boleh mengakses administrasi user ·
sorting hanya kolom yang diizinkan.

## Instalasi
Prasyarat: PHP 8.3+, Composer, MySQL/MariaDB (XAMPP juga bisa).

```bash
composer install
cp .env.example .env
php artisan key:generate
# buat database kosong: inventory_db  (lalu cek DB_* di .env)
php artisan migrate:fresh --seed
php artisan serve
```
Buka http://127.0.0.1:8000 — akun demo: `admin` / `admin123` dan `staff` / `staff123` (ganti sebelum dipakai sungguhan).

Tanpa MySQL: di `.env` set `DB_CONNECTION=sqlite`, hapus baris `DB_HOST` s/d `DB_PASSWORD`, lalu `touch database/database.sqlite`.

## Perintah berguna
```bash
php artisan route:list --except-vendor
php artisan migrate:status
php artisan migrate:fresh --seed
```

## Screenshot
| Login | Dashboard |
|---|---|
| ![](docs/screenshots/04-login.png) | ![](docs/screenshots/07-dashboard-admin.png) |
| Search, filter, sorting | Validasi form (old input + error) |
| ![](docs/screenshots/09-produk-search-filter-sorting.png) | ![](docs/screenshots/12-alur-2-validasi-error-old-input.png) |
| Stok tidak cukup ditolak | Staff → 403 |
| ![](docs/screenshots/24-stok-barang-keluar-melebihi-stok-ditolak.png) | ![](docs/screenshots/35-staff-akses-user-ditolak-403.png) |

![route:list](docs/screenshots/03-artisan-route-list.png)
