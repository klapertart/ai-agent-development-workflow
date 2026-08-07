---
name: impl-plan
description: Gunakan skill ini ketika user ingin membuat implementation plan untuk sebuah issue, memecah issue menjadi task-task kecil, atau merancang langkah-langkah implementasi secara detail. Trigger: "buat impl plan", "implementation plan", "pecah jadi task", "IMPL-".
---

# Implementation Plan Skill — Tahap 2 (AI Pintar)

## Goal
Memecah issue menjadi task-task atomic yang bisa dikerjakan AI murah dalam 1 sesi, lengkap dengan latar belakang analitik, catatan arsitektur, dan verification plan.

## ⚠️ OUTPUT REQUIREMENT — WAJIB DILAKUKAN TANPA MENUNGGU KONFIRMASI USER
**IMMEDIATELY create and write the file `docs/implementation/IMPL_<ISSUE-ID>.md` to disk.**
- Buat folder `docs/implementation/` jika belum ada
- Tulis langsung ke file — JANGAN hanya tampilkan di chat
- JANGAN tanya "apakah saya boleh membuat file?" — langsung buat
- JANGAN tawarkan tombol Proceed untuk pembuatan file ini

## Instructions

1. **Baca konteks wajib sebelum mulai:**
   - `AGENTS.md` — rules dan stack yang harus diikuti
   - `DECISIONS.md` — keputusan arsitektur yang TIDAK boleh di-override
   - `ISSUES.md` section yang relevan — issue yang akan diimplementasi

2. **Pecah menjadi task-task ATOMIC.** Setiap task harus:
   - Selesai dalam 1 sesi AI (estimasi < 200 baris kode)
   - Punya definisi "done" yang jelas dan terverifikasi
   - Tidak ambigu — spesifikasikan nama class, method, package
   - JANGAN melakukan bulk processing berlebihan. Proses maksimal 1 hingga 5 issue per sesi pembuatan plan agar blok instruksi kode tetap spesifik dan detail.

3. **Format yang WAJIB DIIKUTI untuk setiap file Implementation Plan:**
   Gunakan struktur lengkap di bawah ini:

   ```markdown
   # Implementation Plan: [ISSUE-ID] — [Judul Issue]

   **Tanggal Dibuat:** YYYY-MM-DD
   **Tipe:** [Performance / Bugfix / Refactor dsb]
   **Status:** Ready to Implement
   **Referensi Issue:** `ISSUES.md` section `[ISSUE-ID]`

   ---

   ## Latar Belakang & Analisis Root Cause
   - **Root Cause:** [Jelaskan apa yang salah/kurang di kode saat ini]
   - **Dampak:** [Apa akibatnya terhadap performa/sistem]
   - **Solusi Terpilih:** [Pendekatan teknis yang digunakan untuk memperbaiki]

   ---

   ## Keputusan Arsitektur yang Dikonfirmasi
   | Aspek | Keputusan |
   |-------|-----------|
   | [Poin] | [Deskripsi] |

   ---

   ## Task Breakdown

   ---

   ### IMPL-[ISSUE_ID]-T[N]: [Nama Task]
   **Nama Task:** [Sama dengan atas]  
   **Dependency:** [Contoh: IMPL-001-T1 atau Tidak ada]

   #### File yang Diubah
   | Aksi | File |
   |------|------|
   | `[MODIFY/NEW/DELETE]` | `path/ke/file.java` |

   #### Perubahan Detail
   *(WAJIB ada blok FIND dan REPLACE dengan kode konkrit)*
   **🔍 FIND (Kode Lama):**
   ```java
   // snippet lama
   ```
   **✅ REPLACE (Kode Baru):**
   ```java
   // snippet baru
   ```

   #### Definisi Done
   - [ ] Kriteria 1 (bisa diceklist)
   - [ ] Kriteria 2

   ---

   ## Acceptance Criteria Mapping
   | Acceptance Criteria dari ISSUES.md | Task Terkait |
   |---|---|
   | [Kriteria 1] | T1, T2 |

   ---

   ## Verification Plan
   ### Automated Tests
   - [Jelaskan skenario unit testing]
   ### Manual Verification
   - [Jelaskan cara ngetest langsung]

   ---

   ## Draft ADR untuk DECISIONS.md
   ```markdown
   ## XX. [Judul Keputusan Singkat] ([ISSUE-ID])
   - **Problem:** [Masalah awal]
   - **Decision:** [Keputusan perbaikan]
   - **Consequence:** [Efek samping/dampak perbaikan]
   ```
   ```

4. **Jika ada keputusan arsitektur baru** (pattern baru, library baru, struktur package baru):
   - JANGAN putuskan sendiri
   - Tanyakan ke user terlebih dahulu
   - Setelah disetujui, gunakan skill `adr` untuk mencatatnya

5. **Setelah file berhasil ditulis**, konfirmasi ke user:
   ✅ File `docs/implementation/IMPL_<ISSUE-ID>.md` berhasil dibuat.

## Constraints
- DILARANG hanya menampilkan output di chat tanpa menulis ke file
- DILARANG menunggu approval sebelum membuat file implementasi
- DILARANG override keputusan di `DECISIONS.md`
- DILARANG menambah dependency baru tanpa persetujuan eksplisit user
- Scope tiap task harus ketat — tidak boleh ambigu
