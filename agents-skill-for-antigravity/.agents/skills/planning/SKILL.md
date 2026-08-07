---
name: planning
description: Gunakan skill ini ketika user ingin mendiskusikan fitur baru, memulai pengembangan sesuatu yang baru, menganalisis requirement, atau mengidentifikasi risiko teknis sebelum coding dimulai. Trigger: "mau bikin fitur", "diskusi dulu", "planning", "mau tambah", "ada ide".
---

# Planning Skill — Tahap 1 (AI Pintar)

## Goal
Mendefinisikan masalah atau fitur secara jelas sebelum satu baris kode pun ditulis. Output akhir adalah entry baru di `ISSUES.md`.

## Instructions

1. **Baca konteks project terlebih dahulu:**
   - Baca `AGENTS.md` — pahami stack, rules, dan batasan
   - Baca `DECISIONS.md` — pahami keputusan arsitektur yang sudah ada
   - Baca `WORKING_MEMORY.md` — pahami state project saat ini

2. **Lakukan diskusi awal dengan user:**

   Sampaikan ke user:
   > "Sebelum kita mulai, bantu saya klarifikasi beberapa hal:
   > 1. Apa edge case yang perlu dipikirkan?
   > 2. Apa risiko teknis atau dependency yang perlu diperhatikan?
   > 3. Apakah ada alternatif pendekatan yang lebih baik?"

3. **Jangan tulis kode dulu.** Fokus ke pemahaman masalah.

4. **Setelah diskusi selesai**, tawarkan untuk lanjut ke skill `create-issue` untuk menuangkan hasil diskusi ke `ISSUES.md`.

## Constraints
- Jangan langsung membuat implementation plan
- Jangan membuat keputusan arsitektur sendiri — tanyakan ke user
- Jangan tulis kode apapun dalam tahap ini
