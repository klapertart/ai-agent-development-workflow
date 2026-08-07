---
name: coverage
description: Gunakan skill ini untuk mengecek test coverage setelah unit test ditulis, menjalankan JaCoCo report, atau memastikan coverage threshold terpenuhi sebelum lanjut ke integration test. Trigger: "cek coverage", "jacoco", "coverage report", "berapa coverage", "threshold coverage".
---

# Coverage Verification Skill — Tahap 7 (AI Murah)

## Goal
Memastikan coverage threshold terpenuhi sebelum lanjut ke tahap integration test.

## Instructions

1. **Jalankan JaCoCo report:**
   ```
   mvn jacoco:report -pl <module-name>
   ```

2. **Baca laporan** di `target/site/jacoco/index.html` dan laporkan:
   ```
   📊 Coverage Report:
   - Overall Line Coverage: [X]%
   - Overall Branch Coverage: [X]%

   ⚠️ Class di bawah threshold:
   - [ClassName]: line=[X]%, branch=[X]%
   ```

3. **Threshold yang harus dipenuhi:**
   - Line coverage: minimal **80%**
   - Branch coverage: minimal **70%**

4. **Jika ada class di bawah threshold:**
   - Identifikasi method mana yang belum ter-cover
   - Tambahkan test yang kurang menggunakan skill `unit-test`
   - Ulangi cek coverage sampai threshold terpenuhi

5. **Jika threshold terpenuhi:**
   - Konfirmasi ke user: *"Coverage ✅ terpenuhi. Siap lanjut ke integration test."*

## Constraints
- Jangan lanjut ke integration test jika threshold belum terpenuhi
- Jangan manipulasi test hanya untuk menaikkan coverage — test harus meaningful
