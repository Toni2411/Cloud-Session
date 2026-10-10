# CLAUDE.md

Selalu balas dalam Bahasa Indonesia.

## Cara kerja

Untuk setiap tugas coding, pakai skill `ponytail` (level `full`): perubahan terkecil yang menyelesaikan tugas secara utuh, tanpa fitur atau abstraksi yang tidak diminta.

Untuk fitur baru atau perubahan besar, ikuti urutan ini. Tugas kecil (typo, perbaikan satu baris) boleh langsung dikerjakan.

1. `/grill-me` — tanya jawab sampai kita sepaham. Jangan mulai menulis kode sebelum saya konfirmasi.
2. `/to-spec` — rangkum hasil diskusi jadi spec tertulis.
3. `/to-tickets` — pecah spec jadi tiket kecil yang berurutan.
4. `/implement` — kerjakan tiket satu per satu: tes dulu (`tdd`), lalu review (`ponytail-review`), lalu commit.

Skill lain:
- `/ponytail-review` — review perubahan (diff, commit, atau PR).
- `/ponytail-audit` — audit seluruh repo: bug, keamanan, kode yang bisa dihapus.
- `/ponytail-debt` — daftar semua komentar `shortcut:` yang masih jadi utang.
- `system-design-checklist` — dipakai otomatis saat membangun atau mereview backend, API, dan database.

Catatan riset: `riset/aiwithaidityas/RANGKUMAN.md` (rangkuman 66 video TikTok @aiwithaidityas).

Semua skill ada di `.claude/skills/`. Sumber dan lisensinya: [mattpocock/skills](https://github.com/mattpocock/skills) dan [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) (MIT, lihat `.claude/skills/_licenses/`). Di skill `implement`, langkah review diganti dari `code-review` ke `ponytail-review`.

## Agent skills

### Issue tracker

Issue dan spec disimpan sebagai file markdown di `.scratch/`. See `docs/agents/issue-tracker.md`.

### Domain docs

Single-context: satu `GLOSSARY.md` dan `docs/adr/` di root repo. See `docs/agents/domain.md`.
