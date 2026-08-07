---
name: integration-test
description: Gunakan skill ini untuk menjalankan integration test setelah semua task dan unit test selesai, memastikan fitur baru tidak merusak flow yang sudah ada, atau menjalankan full test suite. Trigger: "integration test", "mvn verify", "full test", "cek tidak ada yang rusak", "regression test".
---

# Integration Test Skill — Tahap 8 (AI Murah)

## Goal
Memastikan implementasi baru tidak merusak fitur yang sudah ada (regression check).

## Instructions

1. **Jalankan full test suite:**
   ```
   mvn verify
   ```

2. **Analisis hasil:**

   **Jika semua test pass:**
   ```
   ✅ Integration test PASS
   - Total test: [N]
   - Pass: [N]
   - Fail: 0
   ```
   Update `WORKING_MEMORY.md` dengan status: `Integration Test ✅ PASS untuk [ISSUE-ID]`

   **Jika ada test yang fail:**
   - Identifikasi: apakah fail dari kode BARU atau kode LAMA?
   - Jika dari **kode lama** (yang tidak kamu ubah): **HENTIKAN — laporkan ke user, jangan fix tanpa konfirmasi**
   - Jika dari **kode baru**: identifikasi root cause, fix, dan re-run

3. Informasikan ke user untuk lanjut ke skill `security-scan` jika semua pass.

## Constraints
- JANGAN fix test yang fail dari kode lama tanpa konfirmasi user
- JANGAN skip langkah ini sebelum merge
- Semua test harus hijau sebelum lanjut ke security scan
