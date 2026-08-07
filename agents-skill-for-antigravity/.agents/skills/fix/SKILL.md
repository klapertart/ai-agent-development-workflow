---
name: fix
description: Gunakan skill ini ketika user ingin memperbaiki temuan dari code review, ada item ❌ Must Fix di hasil review, atau setelah review selesai dan perlu perbaikan. Trigger: "fix review", "perbaiki temuan", "ada must fix", "fixing", "perbaiki yang merah".
---

# Fix Review Findings Skill — Tahap 6 (AI Murah)

## Goal
Memperbaiki semua temuan ❌ dari hasil review secara terstruktur dan traceable.

## Instructions

1. **Baca `docs/review/REVIEW_<ISSUE-ID>.md`** — identifikasi semua item ❌ (Must Fix).

2. **Untuk item ⚠️ (Minor Issue):**
   - Tanyakan ke user: *"Apakah item ⚠️ [nama item] perlu diperbaiki sekarang atau dicatat sebagai tech debt?"*
   - Tunggu jawaban sebelum lanjut

3. **Untuk setiap perbaikan ❌, laporkan dengan format:**
   ```
   ### Fix: [nama item review]

   **Sebelum:**
   ```java
   // kode lama
   ```

   **Sesudah:**
   ```java
   // kode baru
   ```

   **Alasan:** [mengapa perubahan ini memperbaiki masalahnya]
   ```

4. **Setelah semua ❌ selesai:**
   - Update status di `REVIEW_<ISSUE-ID>.md` menjadi `"Fixed - Pending Re-review"`
   - Informasikan ke user untuk menjalankan skill `rereview`

## Constraints
- Jangan fix item ⚠️ tanpa konfirmasi user
- Setiap perbaikan harus menunjukkan before/after yang jelas
- Jangan mengubah file di luar scope fix yang diminta
