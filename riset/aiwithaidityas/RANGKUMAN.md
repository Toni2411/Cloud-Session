# Rangkuman akun TikTok @aiwithaidityas

66 video (Desember 2025 – Oktober 2026), total sekitar 90 menit. 55 video ditranskripsi otomatis dengan Whisper; 11 sisanya hanya punya caption (lihat bagian akhir). Transkrip lengkap per video ada di [`transkrip/`](transkrip/). Hasil Whisper kadang salah eja istilah ("vape coding" = vibe coding, "cloud" = Claude, "FBP" = FYP).

Pesan utama akunnya: **jangan cuma vibe coding**. Dengan AI, menulis kode jadi murah, jadi nilai seorang engineer pindah ke pemahaman sistem (system design), cara kerja AI (AI engineering), dan cara memberi AI konteks yang tepat.

## 1. Seri "Belajar System Design" (paling bernilai untuk project)

| Topik | Inti pelajaran | Terapkan di project |
|---|---|---|
| Apa itu system design | Rancangan arsitektur untuk memenuhi kebutuhan fungsional dan non-fungsional. Bikin hasil vibe coding scalable. | Tulis kebutuhan non-fungsional (jumlah user, kecepatan) sebelum coding. |
| Performance vs scalability | Cepat untuk 1 user ≠ tetap cepat untuk 1.000 user. | Uji dengan banyak request, bukan cuma klik manual. |
| Parallelization & message queue | Tambah "kasir" (worker) dan satu antrian adil supaya satu tugas berat tidak menahan yang lain. | Tugas berat (kirim email, proses file) masuk queue. |
| Latency vs throughput | Latency = waktu satu proses; throughput = jumlah yang selesai per waktu. Sering saling tukar. | Tentukan mana yang lebih penting untuk fitur itu. |
| Background jobs (2 episode) | Fire-and-forget, dipicu event atau jadwal (cron). Hasil dikembalikan lewat endpoint status (polling), reply queue, push/pub-sub, webhook, atau shared storage (status di DB). | Untuk proses > beberapa detik: job + status di DB + polling atau webhook. |
| Database replication | Master-slave: tulis ke satu DB, baca dari banyak. Master-master: tulis di mana saja, tapi rawan konflik. | Mulai dengan satu DB; read replica kalau baca jadi bottleneck. |
| Caching | Simpan hasil yang sering diminta; masalah utamanya data basi. Strategi: cache-aside, write-through (konsisten, tulis lebih lambat), write-behind (cepat, rawan tidak konsisten). | Default ke cache-aside + waktu kedaluwarsa (TTL). |
| Monitoring | Health → availability/uptime → performance → security → SLA/usage. | Minimal pasang health check + uptime monitor sebelum launch. |
| SQL vs NoSQL | Relasional (PostgreSQL, MySQL, SQLite) untuk data terstruktur; document (MongoDB) untuk skema fleksibel; graph untuk relasi many-to-many rumit. | Default PostgreSQL (Supabase juga PostgreSQL). |
| N+1 query | 1 query daftar + N query per item. Solusi: JOIN atau ambil sekaligus. | Cek kode hasil AI yang query di dalam loop. |
| Race condition | Dua pembeli stok terakhir → stok negatif. Solusi: pessimistic lock, optimistic lock (kolom versi), mutex, idempotency key, antrian. | Update stok/saldo harus atomik; pembayaran pakai idempotency key. |
| Recursion | Fungsi memanggil diri sendiri sampai base case, lalu "unwind". | Dasar untuk soal interview. |
| Chaos engineering | Tentukan kondisi normal → hipotesis → sengaja rusak satu bagian → bandingkan → perbaiki. Waktu yang dihemat AI dipakai untuk menyempurnakan fitur. | Coba matikan layanan pembayaran/API luar: apakah ada timeout jelas dan tanpa transaksi ganda? |
| Git untuk vibe coder | Commit = checkpoint game; branch = save slot untuk coba fitur. | Commit sebelum minta AI mengubah banyak hal. |
| Rate limiter | Serverless (Vercel, Supabase, S3, API LLM) bisa bikin tagihan meledak saat lonjakan. Pasang rate limit per menit, kuota harian, atau throttling. | Wajib di endpoint publik dan endpoint yang memanggil LLM. |
| Sequential ID | ID berurutan (IMG001, IMG002) bisa ditebak → orang lain mengakses data user lain (BOLA). Solusi: cek otorisasi + UUID. | Selalu cek kepemilikan di server; ID publik pakai UUID. |
| Soft delete | Kolom `deleted_at` alih-alih hapus permanen: bisa dipulihkan, ada jejak audit. Kekurangan: query lebih rumit, storage, privasi. Bisa dikombinasi retention period (hard delete setelah 30 hari). | Pakai untuk data penting user; hard delete kalau diwajibkan privasi. |

## 2. Seri "Belajar AI Engineering"

| Topik | Inti pelajaran |
|---|---|
| Inference | Training → fine-tuning → inference (memakai model untuk data baru). Memanggil API LLM = inference. |
| Embeddings | Teks/gambar diubah jadi vektor angka; makna mirip = vektor berdekatan (cat ≈ kitten, jauh dari airplane). |
| Vector database | Semantic search: cari berdasarkan makna, bukan kata persis. Dasar RAG. |
| Context engineering | Lebih penting dari prompt engineering. Menurut Anthropic: system prompt seimbang (pakai struktur/XML), tools yang tidak tumpang tindih, contoh secukupnya, biarkan agent mencari konteks sendiri, compaction untuk tugas panjang, catatan di file markdown sebagai memori, pecah ke sub-agent. |
| KV caching | LLM memproses per token; hasil hitungan token sebelumnya disimpan supaya tidak dihitung ulang. |
| Quantization | Perkecil presisi angka model (FP32 → FP16 → INT8): lebih hemat memori dan cepat, akurasi turun sedikit. |
| LoRA | Fine-tuning hemat: model asli dibekukan, hanya dua matriks kecil yang dilatih. Gunakan secara etis. |

## 3. Workflow AI dan produktivitas

- **Jangan ulang prompt setiap hari.** Definisikan workflow, standar input/output, lalu jadikan **Projects** atau **Skills** di Claude.
- **Projects** berisi konteks tetap (niche, gaya bicara, contoh terbaik) supaya tidak perlu menjelaskan ulang.
- **Agentic AI** bisa mengerjakan tugas dari awal sampai akhir asalkan standar input/output dan SOP-nya jelas.
- **Konsep 3A** untuk memilih apa yang didelegasikan: *Automation* (rutin, jalan sendiri), *Augmentation* (AI membantu, keputusan tetap di manusia), *Agency* (beri tujuan dan batasan, AI mengerjakan sampai selesai).
- **AI butuh data akurat.** Untuk data saham IDX, sambungkan Claude ke MCP sectors.app, jangan percaya angka dari ingatan model.
- **Integrasi AI end-to-end** (full stack): beri AI "visibilitas" lewat monorepo, MCP database (Postgres/Supabase), akses log (`docker logs`, Railway/Vercel CLI), dan monitoring (Grafana/OpenTelemetry), supaya AI mengumpulkan konteks sendiri.
- **Panduan full stack 2026:** data dulu (data modeling, internal DB, arsitektur; PostgreSQL cukup untuk 80% kasus) → backend/frontend (Next.js untuk mulai; dia sendiri pakai React + TanStack Router dan Go) → DevOps/CI-CD (Docker dan Linux dasar).
- **Desain web tanpa "AI slop":** cari referensi di Dribbble (populer, sebulan terakhir) → pisahkan background → generate versi pertama di Google AI Studio dari gambar → animasikan background → jadikan video latar.
- **Daftar tools:** tulisan panjang (Claude/ChatGPT), rangkum dokumen (Claude/NotebookLM), rangkum YouTube (NotebookLM/Gemini), riset bersumber (Perplexity), terjemahan (Claude/DeepL), web sederhana (Lovable/Replit), aplikasi kompleks (Claude Code).

## 4. Karier, bisnis, dan konten

- **Portfolio yang dilirik** di era AI: project yang benar-benar sulit (banyak cerita untuk interview) atau project yang punya user nyata. Toy project tidak lagi bernilai.
- **Soal interview** yang dibahas: FizzBuzz, Valid Anagram (sort lalu bandingkan), Top K Frequent Elements (bucket sort), Fibonacci (rekursi), Two Sum (simpan angka yang sudah dilihat).
- **Funnel SaaS:** konten organik (murah, butuh viral), paid ads (cepat, butuh kelola cash flow), afiliasi (butuh produk bagus); bisa di-outsource. Trust dan retensi user sama pentingnya.
- **Untuk pemilik bisnis:** langganan AI sekitar Rp350 ribu bisa menghemat ±1 jam kerja repetitif per karyawan per hari (setara ±Rp1 juta per bulan untuk gaji Rp8 juta). Yang dibutuhkan karyawan yang mau mencoba, bukan developer.
- **Konten:** fokus berbagi ilmu ternyata justru menaikkan views.

## 5. Yang sudah diterapkan di repo ini

- Skill [`system-design-checklist`](../../.claude/skills/system-design-checklist/SKILL.md) merangkum bagian 1 jadi checklist yang dipakai Claude saat membangun atau mereview backend.
- Bagian context engineering sudah sejalan dengan setup repo ini: `CLAUDE.md`, skills, dan catatan markdown di `.scratch/`.

## Video tanpa transkrip (11)

Audio TikTok untuk video ini berupa musik latar, dan file videonya butuh login untuk diunduh:

- 2025-12-23 · 1 Hari 1% Ngoding Lebih Baik: FizzBuzz
- 2026-01-09 · Hasil vibe coding 7 jam nonstop (2 video)
- 2026-03-04 · Anak kampung bisa kerja remote?
- 2026-05-12 · Telat atau nggak belajar AI sekarang?
- 2026-05-16 · Prompt engineering ke workflow engineering
- 2026-06-29 · Visualisasi data dengan motion di Claude Design (2 video)
- 2026-07-02 · Kebanyakan nonton influencer AI
- 2026-07-04 · Minta koneksi/network
- 2026-10-09 · Salting (repost dari @programmergrind)
