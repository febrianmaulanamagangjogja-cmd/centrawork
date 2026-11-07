====================================================================
PANDUAN LENGKAP INSTALASI & MENJALANKAN SISTEM CENTRA WORK
Edisi: SaaS Multi-Tenant (Enterprise)
====================================================================

Sistem ini terdiri dari dua bagian utama:
1. Backend (Node.js, Express, Prisma, MySQL, Socket.io)
2. Frontend (React, Ionic, Vite)

--------------------------------------------------------------------
A. PERSYARATAN SISTEM (PREREQUISITES)
--------------------------------------------------------------------
Sebelum menjalankan sistem, pastikan komputer/server Anda telah menginstal:
1. Node.js (Disarankan versi 18.x atau terbaru)
2. MySQL Server (Bisa menggunakan XAMPP, MAMP, atau Laragon)
3. Visual Studio Code (Text Editor)

--------------------------------------------------------------------
B. LANGKAH 1: PERSIAPAN DATABASE
--------------------------------------------------------------------
1. Nyalakan modul MySQL (contoh: via Control Panel XAMPP).
2. Buka phpMyAdmin di browser (biasanya http://localhost/phpmyadmin).
3. Buat database kosong baru dengan nama: centrawork_db

--------------------------------------------------------------------
C. LANGKAH 2: KONFIGURASI BACKEND
--------------------------------------------------------------------
1. Buka folder proyek Backend (centrawork-backend) di VS Code.
2. Buka Terminal di VS Code, lalu jalankan perintah instalasi modul:
   npm install

3. Buat file baru bernama `.env` di root folder backend, dan isi dengan konfigurasi berikut:
   DATABASE_URL="mysql://root:@localhost:3306/centrawork_db"
   JWT_SECRET="centrawork_super_secret_key_123"
   PORT=5000
   USERNAME=root
   PASSWORD=
   (Catatan: Sesuaikan username dan password dengan pengaturan MySQL Anda).

4. Dorong skema tabel ke database dengan perintah:
   npx prisma db push
   npx prisma generate

5. SUNTIKKAN AKUN MASTER (VENDOR):
   Buka kembali phpMyAdmin, pilih database centrawork_db, masuk ke tab SQL, lalu jalankan perintah ini untuk membuat akun Manajemen Pusat:

   INSERT INTO `Role` (`id`, `nama_role`, `level_akses`, `company_id`) VALUES
   (1, 'Manajemen', 0, NULL),
   (2, 'Super Admin', 1, NULL),
   (3, 'Super HR', 2, NULL),
   (4, 'HR Asisten', 4, NULL);

   INSERT INTO `User` (`id`, `role_id`, `company_id`, `nama_lengkap`, `email`, `password`, `status_aktif`, `created_at`) 
   VALUES (UUID(), 1, NULL, 'Vendor Pusat', 'manajemen@centrawork.com', '$2a$10$w095oPIfM10hL64b.p4s4ejB8kXjT5lM7JkH.A3N/2x7eNfH4Xp1q', 1, NOW());

6. Jalankan server Backend dengan perintah:
   npm run dev
   (Biarkan terminal ini tetap menyala).

--------------------------------------------------------------------
D. LANGKAH 3: KONFIGURASI FRONTEND
--------------------------------------------------------------------
1. Buka folder proyek Frontend (centrawork) di terminal baru VS Code.
2. Instal semua dependensi React & Ionic dengan perintah:
   npm install

3. Jalankan aplikasi Frontend dengan perintah:
   npm run dev
   (Atau `ionic serve` jika menggunakan Ionic CLI).

4. Aplikasi akan otomatis terbuka di browser (biasanya di http://localhost:8100 atau http://localhost:5173).

--------------------------------------------------------------------
E. PANDUAN PENGGUNAAN PERTAMA (ONBOARDING)
--------------------------------------------------------------------
1. LOGIN SEBAGAI VENDOR (MANAJEMEN SAAS)
   - Buka aplikasi di browser.
   - Email: manajemen@centrawork.com
   - Password: 123456
   - Di sini Anda bisa mendaftarkan perusahaan/klien baru dan membuatkan akun Super Admin untuk mereka.

2. LOGIN SEBAGAI KLIEN (PENYEWA)
   - Setelah membuat klien baru di Dashboard Manajemen, silahkan Logout.
   - Login menggunakan Email dan Password klien yang baru saja dibuat.
   - Klien (Super Admin) akan masuk ke ruang isolasi mereka sendiri, bisa mengganti logo, mengatur masa sewa, membuat divisi, dan menambah karyawan.

--------------------------------------------------------------------
F. TROUBLESHOOTING UMUM
--------------------------------------------------------------------
- Jika muncul "Cannot read properties of undefined" pada Frontend, pastikan server Backend (port 5000) sedang menyala.
- Jika database error (P2002/P2025), cobalah reset database dengan menjalankan "npx prisma db push --force-reset" (Peringatan: ini akan menghapus semua data), lalu ulangi injeksi SQL akun Master.
- Fitur robot otomatis (Cron Job) akan membatalkan tugas yang lewat tenggat waktu setiap 1 menit. Pastikan backend tidak dimatikan agar fitur ini bekerja maksimal.

====================================================================
Hak Cipta (c) 2026 - Dikembangkan secara eksklusif untuk Centra Work.
====================================================================