# X RPL Dwisaka

Website kelas X RPL (Dwisaka). Tidak ada hubungannya dengan XRP Ledger: nama repo `xrpl` berarti "X RPL".

Bagian depannya berisi berita, galeri, info lomba antar kelas, dan daftar anggota beserta pembayaran kas. Admin menyetujui permintaan pembayaran kas yang masuk. Semua data disimpan di file Excel yang dibaca dan ditulis oleh server Express.

## Fitur

- Login dan daftar akun, dengan peran admin dan user.
- Halaman user: tentang kelas, lomba yang akan datang, galeri, berita terbaru (dengan halaman detail), dan daftar anggota dengan jabatan dan jumlah bayar kas.
- Dashboard admin: total anggota, total kas, daftar permintaan pembayaran yang menunggu, dan tombol setujui.

## Arsitektur

```
ui/                    Next.js 15 + React 19 + Tailwind (pages router)
  src/pages/index.tsx      login dan daftar
  src/pages/users/         beranda user, berita, daftar anggota
  src/pages/admin/         dashboard admin dan form bayar kas
  middleware.ts, roles.ts  pembatasan halaman per peran
backend/node/
  server/server.ts     API Express di port 5000
  data/*.xlsx          akun, data anggota, berita, draft pembayaran
```

Endpoint server:

| Endpoint | Isi |
|---|---|
| `POST /login`, `POST /register` | Akun, password di-hash dengan bcrypt |
| `GET /data` | Data anggota dan kas |
| `GET /berita` | Berita |
| `GET /draft`, `POST /approve` | Permintaan pembayaran dan persetujuannya |
| `POST /update_data` | Ubah data anggota |

Server memakai helmet, rate limit, dan log dengan winston.

## Menjalankan

Butuh Node.js 18 atau lebih baru. Jalankan server dan UI di dua terminal:

```bash
# terminal 1: API
cd backend/node/server
npm install
npm start            # http://localhost:5000

# terminal 2: web
cd ui
npm install
npm run dev          # http://localhost:3000
```

Akun disimpan di `backend/node/data/demo.xlsx`. Buat akun baru lewat form daftar.

UI memanggil API di `http://localhost:5000`, jadi versi GitHub Pages hanya menampilkan halaman tanpa data.
