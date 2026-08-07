---
name: security-scan
description: Gunakan skill ini untuk menjalankan security scan sebelum merge, mengecek vulnerability di kode baru, atau menjalankan pipeline security sesuai SCAN_IMPLEMENTATION_PLAN.md. Trigger: "security scan", "scan keamanan", "cek vulnerability", "sebelum merge", "SCAN_IMPLEMENTATION_PLAN".
---

# Security Scan Skill — Tahap 9 (AI Murah)

## Goal
Menjalankan pipeline security scan sesuai prosedur baku sebelum kode di-merge.

## Instructions

1. **Baca `SCAN_IMPLEMENTATION_PLAN.md`** — ikuti setiap langkah secara berurutan, jangan skip.

2. **Untuk setiap tool dalam pipeline:**
   - Jalankan sesuai perintah yang ada di `SCAN_IMPLEMENTATION_PLAN.md`
   - Catat semua temuan

3. **Jika ada temuan HIGH atau CRITICAL:**
   - **HENTIKAN PROSES SEGERA**
   - Laporkan ke user sebelum lanjut:
     ```
     🚨 SCAN DIHENTIKAN
     Ditemukan [severity]: [deskripsi]
     File: [path]
     Baris: [N]
     Rekomendasi: [saran fix]

     Menunggu konfirmasi user sebelum lanjut.
     ```

4. **Simpan semua temuan** (termasuk LOW dan MEDIUM) ke `docs/review/SECURITY_<ISSUE-ID>.md` dengan format:
   ```
   - Tool: [nama tool]
   - Severity: CRITICAL | HIGH | MEDIUM | LOW
   - File: [path file]
   - Baris: [nomor baris jika ada]
   - Deskripsi: [apa masalahnya]
   - Rekomendasi fix: [saran perbaikan]
   ```

5. **Summary akhir:**
   ```
   📊 Security Scan Summary — [ISSUE-ID]
   - CRITICAL: [N]
   - HIGH: [N]
   - MEDIUM: [N]
   - LOW: [N]

   ✅ Siap merge / ❌ Ada item yang harus difix dulu
   ```

## Constraints
- JANGAN skip langkah apapun dari `SCAN_IMPLEMENTATION_PLAN.md`
- JANGAN lanjut jika ada temuan CRITICAL atau HIGH tanpa konfirmasi user
- JANGAN merge jika security scan belum selesai
