# Smart Locker

Sistem Smart Locker berbasis IoT dengan autentikasi KTP dan PIN. Dokumen rancangan ada di `docs/tahap1-desain-ui-ux.md`.

## Struktur

```
smart-locker/
├── apps/
│   ├── admin/        # Aplikasi Admin (React + Vite), port 5173
│   └── konsumen/     # Aplikasi Konsumen (React + Vite), port 5174
├── backend/          # Node.js + Express, API REST, port 3000
├── packages/shared/  # Konstanta yang dipakai bersama
├── database/
│   └── migrations/   # File SQL migrasi PostgreSQL
└── docs/             # Dokumen rancangan
```

## Menjalankan secara lokal

Syarat: Node.js 20 atau lebih baru, dan PostgreSQL yang sedang berjalan.

1. Instal dependensi dari root proyek:
   ```bash
   npm install
   ```
2. Salin konfigurasi dan sesuaikan `DATABASE_URL`:
   ```bash
   cp .env.example backend/.env
   ```
3. Jalankan migrasi (saat ini belum ada migrasi, perintah ini hanya membuat tabel pencatat):
   ```bash
   npm run migrate
   ```
4. Jalankan backend dan kedua aplikasi di terminal terpisah:
   ```bash
   npm run dev:backend
   npm run dev:admin
   npm run dev:konsumen
   ```
5. Buka `http://localhost:5173` (Admin) dan `http://localhost:5174` (Konsumen). Keduanya harus menampilkan status database.

## Pengujian dan build

```bash
npm test      # tes konfigurasi backend
npm run build # build kedua aplikasi frontend
```

## Endpoint yang sudah tersedia

| Metode | Path | Keterangan |
|---|---|---|
| GET | `/api/health` | Status layanan dan koneksi database. Mengembalikan 503 jika database tidak bisa dijangkau. |

Endpoint lain ditambahkan pada tahap berikutnya sesuai rancangan.

## Aturan keamanan yang sudah diterapkan

- Konfigurasi dibaca dari environment variable, dan server tidak mau jalan tanpa `DATABASE_URL`.
- `helmet` mengatur header keamanan dan `cors` membatasi asal yang boleh mengakses API.
- Body JSON dibatasi 64 KB.
- Pesan error ke klien tidak memuat detail internal.

## Belum diterapkan (tahap berikutnya)

- Autentikasi admin dan konsumen (Tahap 4)
- Penjagaan rute berdasarkan peran (Tahap 4)
- Rate limiting dan pembatasan percobaan PIN (Tahap 8)
- OCR dengan Tesseract dan Dockerfile untuk Railway (Tahap 7 dan 11)
- Tanda tangan perintah buka untuk ESP32 (Tahap 8 dan 9)
