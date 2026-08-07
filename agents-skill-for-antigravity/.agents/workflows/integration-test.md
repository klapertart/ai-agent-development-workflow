---
description: Jalankan full integration test suite setelah semua task selesai. Pastikan tidak ada regression.
---

Jalankan skill `integration-test` untuk issue: $ARGUMENTS

Eksekusi: `mvn verify`
Jika ada test fail dari kode LAMA — hentikan, laporkan ke user, jangan fix tanpa konfirmasi.
Jika ada test fail dari kode BARU — identifikasi root cause dan fix.
Setelah semua pass, update `WORKING_MEMORY.md`: Integration Test ✅ PASS untuk $ARGUMENTS.
