---
description: Jalankan security scan pipeline sebelum merge. Tahap terakhir sebelum kode siap di-merge.
---

Jalankan skill `security-scan` untuk issue: $ARGUMENTS

Ikuti `SCAN_IMPLEMENTATION_PLAN.md` secara berurutan — jangan skip langkah apapun.
Jika ada temuan CRITICAL atau HIGH — HENTIKAN dan laporkan ke user sebelum lanjut.
Tulis semua temuan ke `docs/review/SECURITY_$ARGUMENTS.md`.
Berikan summary: total temuan per severity dan status siap merge atau tidak.
