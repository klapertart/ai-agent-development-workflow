---
name: wrap-session
description: Gunakan skill ini di akhir setiap sesi coding untuk menyimpan state progress, update WORKING_MEMORY.md, atau ketika user akan menutup IDE / mengakhiri sesi kerja. Trigger: "wrap up", "akhiri sesi", "simpan progress", "tutup dulu", "lanjut besok", "update working memory".
---

# Wrap Session Skill — Tahap 3 (AI Murah)

## Goal
Memastikan `WORKING_MEMORY.md` selalu mencerminkan state terkini project sehingga sesi berikutnya bisa dilanjutkan tanpa kehilangan konteks.

## Instructions

1. **Update `WORKING_MEMORY.md`** dengan informasi berikut:

   ```markdown
   # Working Memory
   **Last Updated:** [tanggal & waktu sekarang]

   ## Sesi Terakhir
   **Task yang selesai:**
   - [IMPL-ID-TN] [nama task] — ✅ Done / 🔄 Partial

   **File yang dibuat/dimodifikasi:**
   - `path/File.java` — [apa yang diubah]

   **Keputusan kecil yang dibuat:**
   - [jika ada, atau "Tidak ada"]

   ## Blockers / Open Questions
   - [pertanyaan atau hambatan yang perlu diklarifikasi, atau "Tidak ada"]

   ## Next Session — Task Selanjutnya
   - [IMPL-ID-TN] [nama task berikutnya yang harus dikerjakan]
   ```

2. **Konfirmasi ke user** bahwa `WORKING_MEMORY.md` sudah diupdate dan sesi aman untuk ditutup.

## Constraints
- WAJIB dijalankan setiap akhir sesi tanpa pengecualian
- Jangan overwrite history sebelumnya — append atau update section yang relevan
- Next session task harus spesifik dengan ID task yang jelas
