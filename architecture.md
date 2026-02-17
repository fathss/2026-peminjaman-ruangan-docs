# Dokumentasi Arsitekstur Sistem
Dokumen ini menjelaskan struktur teknis, aliran data, dan keputusan arsitektur yang diambil dalam pengembangan sistem Peminjaman Ruangan.

## 1. High-Level Overview
Sistem ini menggunakan arsitektur Client-Server yang terpisah, di mana Frontend React berkomunikasi dengan Backend ASP.NET melalui RESTful API.

#### Komponen Utama:
* **Frontend (React + TypeScript):** Aplikasi Single Page Application (SPA) yang menangani antarmuka pengguna, manajemen state (Context API), dan validasi input sisi klien.
* **Backend (ASP.NET 10 Core):** Server API utama yang menangani logika bisnis, keamanan, dan interaksi database.
* **Database (PostgreSQL):** Penyimpanan data relasional untuk User, Room, Booking, dan History.
* **Authentication (JWT):** Mekanisme keamanan stateless untuk mengamankan akses API.

## 2. Alur Autentikasi (JWT Flow)
Sistem menggunakan **JSON Web Token (JWT)** untuk memastikan setiap permintaan API berasal dari pengguna yang sah.

1. **Login:** User mengirim kredensial ke `/api/auth/login`.
2. **Validation:** Backend memverifikasi kredensial terhadap database PostgreSQL.
3. **Token Generation:** Jika valid, Backend menghasilkan JWT yang berisi `Claims` (User ID, Role, Username).
4. **Storage:** Frontend menyimpan token di `localStorage`.
5. **Authorized Requests:** Untuk setiap request ke endpoint terproteksi, Frontend mengirim token di Header: `Authorization: Bearer <token>`.
6. **Verification:** Middleware ASP.NET memvalidasi signature token sebelum mengizinkan akses ke Controller.

## 3. Struktur Data & Database (PostgerSQL)
Database dirancang dengan normalisasi untuk menjaga integritas data peminjaman.
* **Users Tables:** Menyimpan data pengguna dan peran.
* **Rooms Tables:** Menyimpan informasi fisik ruangan.
* **RoomBookings Tables:** Tabel transaksi pusat yang mencatat waktu mulai, selesai, dan status.
* **BookingStatusHistories Tables:** Mencatat setiap perubahan status untuk transparansi data.

## 4. Komunikasi Data (API Layer)
* **Format:** Semua pertukaran data menggunakan format &**JSON**.
* **DTO (Data Transfer Objects):** Digunakan di sisi Backend untuk memisahkan model database dengan data yang dikirim ke Frontend. Hal ini mencegah kebocoran data sensitif.
* **Axios Instance:** Frontend menggunakan instance Axios terpusat dengan konfigurasi `BASE_URL` dari environment variable Vite dan interceptor untuk menyisipkan token JWT secara otomatis.

## 5. Diagram Alur Peminjaman
1. **Request:** User memilih ruangan dan waktu melalui form React.
2. **Business Logic:** ASP.NET mengecek apakah `RoomID` tersedia pada jam tersebut di PostgreSQL.
3. **Persistence:** Jika tersedia, data disimpan dengan status `Pending`.
4. **Action:** Admin mengubah status melalui endpoint `/approve` atau `/reject`.
5. **Manual Action:**
    * **Cancel:** User dapat membatalkan peminjaman (status menjadi `Cancelled`) selama status belum `OnGoing`.
    * **Complete Manual:** User dapat mengakhiri peminjaman lebih awal dari `EndTime` yang direncanakan melalui tombol "Selesaikan" di Dashboard.