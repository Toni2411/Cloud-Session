---
name: system-design-checklist
description: Checklist system design untuk fitur backend, API, database, pembayaran, upload, atau apa pun yang akan dipakai banyak user. Use when building or reviewing server-side code, database schemas, endpoints, background jobs, or before deploying a vibe-coded app.
---

# System design checklist

Dirangkum dari seri "Belajar System Design" @aiwithaidityas (lihat `riset/aiwithaidityas/RANGKUMAN.md`).

Jalankan checklist ini pada kode yang kamu tulis atau review. Hanya angkat poin yang **relevan dengan perubahan ini**; jangan menambahkan infrastruktur yang tidak dibutuhkan skala project (tetap ikuti `ponytail`). Laporkan poin yang sengaja dilewati dalam satu baris.

## Keamanan dan data

- **Otorisasi per objek**: setiap endpoint yang menerima ID mengecek bahwa data itu milik user yang meminta. ID yang terlihat publik memakai UUID, bukan angka berurutan.
- **Race condition**: update stok, saldo, kuota, atau kursi dilakukan atomik (`UPDATE ... WHERE stock > 0`, optimistic lock dengan kolom versi, atau lock). Pembayaran dan request yang bisa terulang memakai idempotency key.
- **Hapus data**: data penting user memakai soft delete (`deleted_at`) dan semua query menyaring `deleted_at IS NULL`. Data yang wajib dihapus demi privasi tetap di-hard delete, atau pakai retention period.

## Performa dan biaya

- **N+1 query**: tidak ada query database di dalam loop; pakai JOIN atau ambil sekaligus.
- **Rate limit**: endpoint publik, login, dan endpoint yang memanggil LLM atau layanan berbayar punya rate limit dan/atau kuota harian, supaya lonjakan request tidak meledakkan tagihan serverless.
- **Caching**: hanya untuk data yang sering dibaca dan jarang berubah. Default cache-aside dengan TTL; tentukan kapan cache dihapus saat data berubah.
- **Pekerjaan lama** (email, proses file, panggilan AI panjang): jadikan background job. Simpan status job di database dan kembalikan hasil lewat polling endpoint status atau webhook.

## Keandalan

- **Panggilan ke layanan luar** punya timeout, retry terbatas (bukan tak terbatas), dan pesan error yang jelas ke user. Pikirkan: apa yang terjadi kalau layanan ini mati? (chaos engineering sederhana).
- **Monitoring** sebelum launch: minimal health check endpoint dan uptime monitor; log error bisa dibaca (dan bisa diberikan ke AI).
- **Database**: default PostgreSQL. Replikasi, sharding, atau NoSQL hanya kalau ada alasan nyata.
- **Git**: commit sebelum perubahan besar oleh AI supaya selalu ada checkpoint.
