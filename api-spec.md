# Dokumentasi API - Peminjaman Ruangan
Semua permintaan API menggunakan Base URL berikut:
`http://localhost:5145/api`
## 1. Endpoints Autentikasi (/auth)
* `POST /auth/login` - Login untuk user dan admin.
* `POST /auth/register` - Membuat akun user baru.

## 2. Endpoints Ruangan (/rooms)
* `GET /rooms` - Mengambil daftar seluruh ruangan.
* `GET /rooms/{id}` - Mengambil informasi ruangan berdasarkan ID.
* `POST /rooms` - **(Admin Only)** Membuat ruangan baru.
* `PUT /rooms/{id}` - **(Admin Only)** Memperbarui informasi ruangan.
* `DELETE /rooms/{id}` - **(Admin Only)** Menghapus ruangan.

## 3. Endpoints Peminjaman (/roombookings)
* `GET /roombookings` - Mengambil semua daftar peminjaman (Admin: Semua, User: Milik sendiri).
* `GET /roombookings/{id}` - Mengambil detail spesifik satu peminjaman.
* `POST /roombookings` - Membuat pengajuan peminjaman ruangan baru.
* `PUT /roombookings/{id}` - Memperbarui data peminjaman.
* `PUT /roombookings/{id}/approve`- **(Admin Only)** Menyetujui pengajuan.
* `PUT /roombookings/{id}/reject` - **(Admin Only)** Menolak pengajuan.
* `PUT /roombookings/{id}/complete` - Menyelesaikan peminjaman secara manual.
* `PUT /roombookings/{id}/cancel` - Membatalkan peminjaman.
* `DELETE /roombookings/{id}` - Menghapus data peminjaman.
