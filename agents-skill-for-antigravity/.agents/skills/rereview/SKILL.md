---
name: rereview
description: Gunakan skill ini setelah fix selesai dan status review sudah "Fixed - Pending Re-review", untuk memverifikasi bahwa perbaikan sudah benar dan tidak menimbulkan masalah baru. Trigger: "re-review", "cek hasil fix", "verifikasi fix", "review ulang", "pending re-review".
---

# Re-review Skill — Tahap 6 (AI Pintar)

## Goal
Memverifikasi bahwa semua item ❌ sudah diperbaiki dengan benar dan tidak menimbulkan masalah baru.

## Instructions

1. **Baca `docs/review/REVIEW_<ISSUE-ID>.md`** — fokus hanya pada item yang sebelumnya ditandai ❌.

2. **Untuk setiap item yang di-fix:**
   - Verifikasi apakah perbaikan sudah sesuai instruksi review sebelumnya
   - Pastikan fix tidak menimbulkan masalah baru (regression)
   - Pastikan fix tidak menyimpang dari `DECISIONS.md` atau `AGENTS.md`

3. **Update status di `REVIEW_<ISSUE-ID>.md`:**
   - Jika semua fix benar → ubah status menjadi `"✅ Approved"`
   - Jika masih ada masalah → tandai ulang sebagai ❌ dengan instruksi yang lebih spesifik

4. **Jika Approved**, informasikan ke user untuk lanjut ke skill `unit-test`.

## Constraints
- Fokus hanya pada item yang sebelumnya ❌ — jangan buka review baru
- Jika ada masalah baru ditemukan, laporkan sebagai temuan terpisah
- Jangan approve jika ada item ❌ yang belum benar-benar diperbaiki
