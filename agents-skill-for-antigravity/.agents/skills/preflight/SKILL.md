---
name: preflight
description: Gunakan skill ini di awal setiap sesi implementasi sebelum mulai coding, ketika user menyebut task ID yang akan dikerjakan, atau ketika memulai sesi baru setelah jeda. Trigger: "mulai coding", "kerjakan task", "lanjut implementasi", "preflight", "IMPL-".
---

# Pre-flight Check Skill — Tahap 3 (AI Murah)

## Goal
Memastikan semua konteks sudah dimuat sebelum mulai coding. Mencegah AI coding tanpa pemahaman rules dan state project.

## Instructions

1. **Baca semua file konteks wajib:**
   - `AGENTS.md` — konfirmasi stack, rules, dan batasan yang berlaku
   - `DECISIONS.md` — konfirmasi keputusan arsitektur yang tidak boleh diubah
   - `WORKING_MEMORY.md` — lihat state sesi sebelumnya dan task yang sedang aktif
   - `docs/implementation/IMPL_<TASK-ID>.md` — pahami task yang akan dikerjakan

2. **Respond dengan checklist berikut:**
   ```
   ✅ Sudah membaca AGENTS.md — [ringkasan rules kunci]
   ✅ Sudah membaca DECISIONS.md — [ringkasan keputusan yang berlaku]
   ✅ Sudah membaca WORKING_MEMORY.md — [state terakhir]
   📋 Task yang akan dikerjakan: [IMPL-ID-TN] — [nama task]
   ⚠️ Potensi konflik / ketidakjelasan: [sebutkan jika ada, atau "Tidak ada"]
   ```

3. **Jangan mulai menulis kode sebelum user konfirmasi.**

4. Jika ada dependency task yang belum selesai — **hentikan dan laporkan ke user**.

## Constraints
- WAJIB dilakukan di awal setiap sesi implementasi tanpa pengecualian
- Jangan skip langkah membaca file konteks
- Jangan mulai coding tanpa konfirmasi dari user
