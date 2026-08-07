---
name: review
description: Gunakan skill ini ketika user ingin mereview kode yang baru diimplementasi, melakukan code review sebelum lanjut ke tahap berikutnya, atau mengecek kualitas implementasi terhadap rubrik standar. Trigger: "review kode", "code review", "cek implementasi", "review task", "review dulu".
---

# Code Review Skill — Tahap 5 (AI Pintar)

## Goal
Mereview hasil implementasi dengan rubrik konsisten. Output: file `docs/review/REVIEW_<ISSUE-ID>.md`.

## Instructions

1. **Identifikasi file yang perlu direview:**
   - Baca `docs/implementation/IMPL_<ISSUE-ID>.md` untuk tahu file mana yang diubah/dibuat

2. **Gunakan rubrik review berikut** (nilai tiap item: ✅ Pass | ⚠️ Minor Issue | ❌ Must Fix):

   **1. CORRECTNESS**
   - Apakah logika sudah sesuai acceptance criteria di `ISSUES.md`?
   - Apakah edge case sudah ditangani?

   **2. ADHERENCE TO AGENTS.md**
   - Stack, naming convention, dan rules diikuti?
   - Ada penyimpangan dari `DECISIONS.md`?

   **3. SECURITY (Spring Boot context)**
   - Input validation sudah ada?
   - Tidak ada hardcoded credential / secret?
   - Rentan SQL injection atau injection lainnya?
   - Authorization check sudah sesuai?

   **4. ERROR HANDLING**
   - Exception ditangani dengan benar?
   - Response error konsisten dengan pattern yang ada?

   **5. PERFORMANCE**
   - Ada N+1 query problem?
   - Ada operasi yang harusnya async tapi dibuat sync?

   **6. TESTABILITY**
   - Kode mudah di-unit test?
   - Ada tight coupling yang menyulitkan testing?

   **7. MAINTAINABILITY**
   - Kode mudah dibaca?
   - Ada magic number / hardcoded string yang harusnya jadi konstanta?

3. **Format output:**
   ```
   ## Summary Verdict
   [✅ Approved | ⚠️ Approved with Notes | ❌ Must Fix Before Proceed]

   ## Temuan per Kategori
   [Detail per rubrik]

   ## Action Items
   [Untuk setiap ❌: instruksi perbaikan yang spesifik dan actionable]
   ```

4. **Simpan ke** `docs/review/REVIEW_<ISSUE-ID>.md`

## Constraints
- Rubrik harus diikuti konsisten setiap kali review
- Jangan lanjut ke tahap berikutnya jika verdict = ❌ Must Fix
- Instruksi perbaikan harus spesifik, bukan generik
