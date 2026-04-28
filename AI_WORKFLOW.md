# AI Agent Development Workflow
> Mature workflow untuk AI-assisted development dengan Spring Boot.
> Gunakan file ini sebagai panduan utama dalam setiap siklus pengembangan.

---

## Prinsip Dasar

| Prinsip | Penjelasan |
|---|---|
| **Model Tiering** | AI Pintar (Opus/Sonnet) untuk thinking, AI Murah (Haiku/smaller) untuk execution |
| **Single Source of Truth** | Semua keputusan arsitektur ada di `DECISIONS.md`, aturan ada di `AGENTS.md` |
| **Atomic Tasks** | Setiap task harus bisa selesai dalam 1 sesi AI, punya definisi "done" yang jelas |
| **Context Persistence** | `WORKING_MEMORY.md` selalu diupdate di awal & akhir setiap sesi |
| **No Silent Drift** | AI murah dilarang override keputusan di `DECISIONS.md` tanpa approval eksplisit |

---

## Struktur File Wajib

```
project-root/
├── AGENTS.md                    # Rules, stack, boundaries (sudah punya)
├── SCAN_IMPLEMENTATION_PLAN.md  # Security scan pipeline (sudah punya)
├── AI_WORKFLOW.md               # File ini — panduan workflow
├── ISSUES.md                    # Backlog fitur/bug yang sudah didefinisikan
├── WORKING_MEMORY.md            # State sesi aktif (diupdate tiap sesi)
├── DECISIONS.md                 # Architecture Decision Records
└── docs/
    └── implementation/
        └── IMPL_<ISSUE_ID>.md   # Implementation plan per issue
    └── review/
        └── REVIEW_<ISSUE_ID>.md # Review result per issue
    └── changelog/
        └── CHANGELOG.md         # Auto-generated dari ISSUES.md
```

---

## Overview Workflow

```
[Mulai Aplikasi / Fitur / Bug Baru]
         │
         ▼
  ┌─────────────┐
  │  TAHAP 1    │  PLANNING — AI Pintar
  │  ISSUES.md  │  Definisikan masalah, scope, acceptance criteria
  └──────┬──────┘
         │
         ▼
  ┌─────────────────────┐
  │      TAHAP 2        │  IMPLEMENTATION PLAN — AI Pintar
  │  IMPL_<ID>.md       │  Pecah jadi atomic tasks, definisikan dependency
  │  + DECISIONS.md     │  Catat keputusan arsitektur
  └──────┬──────────────┘
         │
         ▼
  ┌─────────────────────────────────────────────────────┐
  │   TAHAP 3,4,5,6,7 - LOOP PER TASK                   │
  │                                                     │
  │  Pre-flight Check → Implement → Review → Fix → Test │
  │      (AI Murah)     (AI Murah) (AI Pintar)(AI Murah)│
  └──────┬──────────────────────────────────────────────┘
         │  semua task selesai
         ▼
  ┌─────────────────────┐
  │      TAHAP 8        │  INTEGRATION TEST — AI Murah
  └──────┬──────────────┘
         │
         ▼
  ┌─────────────────────┐
  │      TAHAP 9        │  SECURITY SCAN — AI Murah
  │  (ikuti             │  Jalankan SCAN_IMPLEMENTATION_PLAN.md
  │  SCAN_IMPL_PLAN.md) │
  └──────┬──────────────┘
         │
         ▼
  [Iterasi ke Issue Berikutnya]
```

---

## Tahap 1 — Planning (AI Pintar)

**Tujuan:** Mendefinisikan masalah/fitur secara jelas sebelum satu baris kode pun ditulis.

**Siapa yang mengerjakan:** Kamu + AI Pintar secara kolaboratif.

**Output:** Entry baru di `ISSUES.md`.

---

### Prompt: Diskusi Awal Fitur

```
Saya ingin menambahkan fitur [NAMA FITUR] ke project Spring Boot saya.

Konteks sistem:
- [Jelaskan singkat arsitektur yang sudah ada]
- [Sebutkan entitas/service yang relevan]

Yang ingin saya capai:
- [Deskripsikan tujuan bisnis / teknis]

Sebelum kita mulai, bantu saya klarifikasi:
1. Apa edge case yang perlu dipikirkan?
2. Apa risiko teknis atau dependency yang perlu diperhatikan?
3. Apakah ada alternatif pendekatan yang lebih baik?

Jangan tulis kode dulu. Fokus ke pemahaman masalah.
```

---

### Prompt: Tuangkan ke ISSUES.md

```
Berdasarkan diskusi kita barusan tentang [NAMA FITUR], 
sekarang tuangkan hasilnya ke dalam format ISSUES.md berikut:

Format yang harus diikuti:
---
## [ISSUE-ID] Judul Issue

**Tipe:** Feature | Bug | Refactor | Security  
**Prioritas:** High | Medium | Low  
**Status:** Backlog

### Problem Statement
[Apa masalah yang diselesaikan, dari perspektif bisnis/user]

### Proposed Solution
[Pendekatan teknis yang disepakati]

### Acceptance Criteria
- [ ] Kriteria 1 (testable dan spesifik)
- [ ] Kriteria 2
- [ ] Kriteria N

### Out of Scope
- [Apa yang TIDAK termasuk dalam issue ini]

### Dependencies
- [Issue lain / service / library yang dibutuhkan]

### Estimasi Kompleksitas
Low | Medium | High — [alasan singkat]
---

Pastikan acceptance criteria bisa diverifikasi dengan unit test.
Jangan langsung membuat implementation plan.
```

---

## Tahap 2 — Implementation Plan (AI Pintar)

**Tujuan:** Memecah issue menjadi task-task kecil yang bisa dikerjakan AI murah dalam 1 sesi.

**Siapa yang mengerjakan:** AI Pintar.

**Output:** File `docs/implementation/IMPL_<ISSUE_ID>.md` + update `DECISIONS.md` jika ada keputusan arsitektur baru.

---

### Prompt: Generate Implementation Plan

```
Baca file berikut sebagai konteks wajib:
- AGENTS.md (rules dan stack yang harus diikuti)
- DECISIONS.md (keputusan arsitektur yang sudah dibuat, JANGAN di-override)
- ISSUES.md section [ISSUE-ID] (issue yang akan diimplementasi)

Buat implementation plan untuk issue [ISSUE-ID] dengan ketentuan:

1. Pecah menjadi task-task ATOMIC — setiap task harus:
   - Selesai dalam 1 sesi AI (estimasi < 200 baris kode)
   - Punya definisi "done" yang jelas dan terverifikasi
   - Tidak ambigu — spesifikasikan nama class, method, package

2. Setiap task harus mencantumkan:
   - ID task (IMPL-[ISSUE_ID]-T[N])
   - Nama task
   - File yang akan dibuat/diubah (path lengkap)
   - Definisi done
   - Dependency ke task lain (jika ada)

3. Jika ada keputusan arsitektur baru (pattern baru, library baru, 
   struktur package baru), JANGAN putuskan sendiri — 
   tanyakan ke saya terlebih dahulu.

4. Di akhir, jika ada keputusan yang sudah kita sepakati dalam diskusi ini,
   tambahkan draft ADR untuk DECISIONS.md.

Format output: markdown, simpan ke docs/implementation/IMPL_[ISSUE-ID].md
```

---

### Prompt: Catat Keputusan Arsitektur ke DECISIONS.md

```
Berdasarkan implementation plan yang baru dibuat, 
ada keputusan [NAMA KEPUTUSAN] yang perlu dicatat.

Tambahkan entry baru ke DECISIONS.md dengan format ADR standar:
- Konteks: mengapa keputusan ini perlu dibuat
- Keputusan: apa yang diputuskan
- Konsekuensi: apa implikasinya ke codebase
- Status: Accepted

Ini adalah keputusan yang MENGIKAT — AI yang mengerjakan implementasi
tidak boleh menyimpang dari ini tanpa persetujuan eksplisit.
```

---

## Tahap 3 — Pre-flight Check (AI Murah)

**Tujuan:** Memastikan AI murah punya semua konteks sebelum mulai coding.

**Siapa yang mengerjakan:** AI Murah — WAJIB dilakukan di awal setiap sesi implementasi.

---

### Prompt: Pre-flight Check

```
Sebelum mulai coding, lakukan pre-flight check berikut:

1. Baca AGENTS.md — konfirmasi stack, rules, dan batasan yang berlaku
2. Baca DECISIONS.md — konfirmasi keputusan arsitektur yang tidak boleh diubah
3. Baca WORKING_MEMORY.md — lihat state sesi sebelumnya dan task yang sedang aktif

Setelah membaca semua file di atas, respond dengan:
- ✅ Konfirmasi bahwa kamu sudah membaca semua file
- 📋 Task yang akan dikerjakan sesi ini (sebutkan ID-nya)
- ⚠️ Jika ada ketidakjelasan atau potensi konflik dengan DECISIONS.md, sebutkan SEBELUM mulai coding
- ❌ Jika ada dependency task yang belum selesai, hentikan dan laporkan

Jangan mulai menulis kode sebelum saya konfirmasi.
```

---

### Prompt: Update WORKING_MEMORY di Akhir Sesi

```
Sesi coding kita akan selesai. Update WORKING_MEMORY.md dengan:

1. Task yang selesai dikerjakan sesi ini (dengan status done/partial)
2. File yang dibuat atau dimodifikasi (path lengkap)
3. Keputusan kecil yang dibuat selama coding (jika ada)
4. Blockers atau open questions yang perlu diklarifikasi
5. Task berikutnya yang harus dilanjutkan di sesi berikutnya

Pastikan WORKING_MEMORY.md selalu mencerminkan state terkini project.
```

---

## Tahap 4 — Implementation (AI Murah)

**Tujuan:** Mengeksekusi satu atomic task dari implementation plan.

**Siapa yang mengerjakan:** AI Murah.

---

### Prompt: Implementasi Task

```
Kerjakan task dari file docs/implementation/IMPL_[ISSUE-ID].md.

Constraints WAJIB:
- Ikuti semua rules di AGENTS.md tanpa pengecualian
- Jangan menyimpang dari DECISIONS.md
- Scope ketat: hanya kerjakan yang ada di definisi task ini, tidak lebih
- Jika menemukan sesuatu yang perlu diputuskan di luar scope task, 
  JANGAN putuskan sendiri — tulis sebagai comment TODO dan laporkan ke saya

Struktur kode Spring Boot yang diharapkan:
- Error handling: gunakan pattern yang sudah ada di codebase
- Jangan tambahkan dependency baru tanpa persetujuan eksplisit

Setelah selesai:
1. Tunjukkan semua file yang dibuat/diubah
2. Konfirmasi definisi "done" dari task ini sudah terpenuhi
3. Sebutkan jika ada TODO yang perlu saya review
```

---

## Tahap 5 — Code Review (AI Pintar)

**Tujuan:** Review hasil implementasi AI murah dengan rubrik yang konsisten.

**Siapa yang mengerjakan:** AI Pintar.

**Output:** File `docs/review/REVIEW_<ISSUE_ID>.md`.

---

### Prompt: Code Review

```
Review hasil implementasi task [IMPL-[ISSUE-ID]-T[N]].

File yang perlu direview:
[list file yang diubah/dibuat]

Gunakan rubrik review berikut (nilai: ✅ Pass | ⚠️ Minor Issue | ❌ Must Fix):

1. CORRECTNESS
   - Apakah logika sudah sesuai acceptance criteria di ISSUES.md?
   - Apakah edge case sudah ditangani?

2. ADHERENCE TO AGENTS.md
   - Apakah stack, naming convention, dan rules diikuti?
   - Apakah ada penyimpangan dari DECISIONS.md?

3. SECURITY (Spring Boot context)
   - Input validation sudah ada?
   - Tidak ada hardcoded credential / secret?
   - SQL injection / injection vulnerability lainnya?
   - Authorization check sudah sesuai?

4. ERROR HANDLING
   - Apakah exception ditangani dengan benar?
   - Apakah response error sudah konsisten dengan pattern yang ada?

5. PERFORMANCE
   - Apakah ada N+1 query problem?
   - Apakah ada operasi yang harusnya async tapi dibuat sync?

6. TESTABILITY
   - Apakah kode mudah di-unit test?
   - Apakah ada tight coupling yang membuat testing sulit?

7. MAINTAINABILITY
   - Apakah kode mudah dibaca?
   - Apakah ada magic number / hardcoded string yang seharusnya jadi konstanta?

Output format:
- Summary verdict: ✅ Approved | ⚠️ Approved with Notes | ❌ Must Fix Before Proceed
- Detail temuan per kategori
- Jika ada ❌, berikan instruksi perbaikan yang spesifik dan actionable
- Simpan ke docs/review/REVIEW_[ISSUE-ID].md
```

---

## Tahap 6 — Fixing (AI Murah)

**Tujuan:** Memperbaiki semua temuan ❌ dari hasil review.

**Siapa yang mengerjakan:** AI Murah.

---

### Prompt: Fix Review Findings

```
Baca docs/review/REVIEW_[ISSUE-ID].md.

Perbaiki semua item yang ditandai ❌ (Must Fix).
Untuk item ⚠️ (Minor Issue), tanyakan ke saya apakah perlu diperbaiki sekarang atau dicatat sebagai tech debt.

Untuk setiap perbaikan:
1. Sebutkan item review yang diperbaiki
2. Tunjukkan kode sebelum dan sesudah (diff style)
3. Jelaskan singkat mengapa perubahan ini memperbaiki masalahnya

Setelah semua ❌ selesai, update status di REVIEW_[ISSUE-ID].md menjadi "Fixed - Pending Re-review".
```

---

### Prompt: Re-review Setelah Fix (AI Pintar)

```
Lakukan re-review untuk item-item yang sudah difix di docs/review/REVIEW_[ISSUE-ID].md.

Fokus hanya pada item yang sebelumnya ditandai ❌.
Verifikasi apakah perbaikan sudah benar dan tidak menimbulkan masalah baru.

Jika semua sudah fix, update status review menjadi "✅ Approved".
Jika masih ada masalah, tandai ulang dan berikan instruksi yang lebih spesifik.
```

---

## Tahap 7 — Unit Test (AI Murah)

**Tujuan:** Memastikan semua kode yang ditulis punya test coverage yang memadai.

**Siapa yang mengerjakan:** AI Murah.

---

### Prompt: Generate Unit Tests

```
Buat unit test untuk semua kode yang diimplementasi di task [IMPL-[ISSUE-ID]-T[N]].

Stack testing yang digunakan (sesuai AGENTS.md):
- JUnit 5
- Mockito untuk mocking
- AssertJ untuk assertions
- @SpringBootTest hanya jika benar-benar perlu (prefer unit test murni)

Requirement test:
1. Setiap public method harus punya minimal 1 happy path test
2. Cover semua branch kondisi (if/else, switch)
3. Cover edge case yang didefinisikan di ISSUES.md
4. Test nama harus deskriptif: should_[expected]_when_[condition]
5. Gunakan @DisplayName untuk readability

Coverage target:
- Line coverage: minimal 80%
- Branch coverage: minimal 70%

Setelah selesai, jalankan: mvn test -pl [module] dan pastikan semua test hijau.
Tunjukkan output test result.
```

---

### Prompt: Verifikasi Test Coverage

```
Jalankan perintah berikut untuk cek coverage:
mvn jacoco:report

Kemudian buka laporan di target/site/jacoco/index.html dan laporkan:
- Overall line coverage
- Overall branch coverage
- Class mana yang coverage-nya di bawah 80%

Jika ada class yang di bawah threshold, tambahkan test yang kurang.
Jangan lanjut ke tahap berikutnya sampai threshold terpenuhi.
```

---

## Tahap 8 — Integration Test (AI Murah)

**Tujuan:** Memastikan fitur baru tidak merusak flow end-to-end yang sudah ada.

---

### Prompt: Integration Test

```
Jalankan integration test untuk memverifikasi bahwa implementasi [ISSUE-ID] 
tidak merusak fitur yang sudah ada.

Langkah:
1. Jalankan full test suite: mvn verify
2. Jika ada test yang fail yang BUKAN dari kode yang baru ditulis, 
   laporkan ke saya — JANGAN fix tanpa konfirmasi
3. Jika ada test yang fail karena kode baru, identifikasi root cause-nya

Jika semua test pass, konfirmasi bahwa integration test selesai 
dan update WORKING_MEMORY.md.
```

---

## Tahap 9 — Security Scan (AI Murah)

**Tujuan:** Menjalankan pipeline security scan sesuai prosedur baku.

---

### Prompt: Jalankan Security Scan

```
Jalankan security scan mengikuti file SCAN_IMPLEMENTATION_PLAN.md secara berurutan.

Untuk setiap tool dalam pipeline:
1. Jalankan sesuai perintah yang ada di SCAN_IMPLEMENTATION_PLAN.md
2. Jangan skip langkah apapun
3. Jika ada temuan dengan severity HIGH atau CRITICAL, HENTIKAN dan laporkan ke saya sebelum lanjut
4. Catat semua temuan (termasuk LOW dan MEDIUM) ke docs/review/SECURITY_[ISSUE-ID].md

Format laporan temuan:
- Tool: [nama tool]
- Severity: CRITICAL | HIGH | MEDIUM | LOW
- File: [path file]
- Baris: [nomor baris jika ada]
- Deskripsi: [apa masalahnya]
- Rekomendasi fix: [saran perbaikan]

Setelah semua scan selesai, berikan summary:
- Total temuan per severity
- Item yang perlu segera difix sebelum merge
```

---

## Quick Reference: Kapan Pakai Model Mana

| Tugas | Model |
|---|---|
| Diskusi masalah & klarifikasi requirements | **Pintar** |
| Membuat ISSUES.md entry | **Pintar** |
| Membuat implementation plan | **Pintar** |
| Code review | **Pintar** |
| Re-review setelah fix | **Pintar** |
| Evaluasi keputusan arsitektur baru | **Pintar** |
| Pre-flight check | **Murah** |
| Implementasi kode | **Murah** |
| Fixing review findings | **Murah** |
| Unit test | **Murah** |
| Integration test | **Murah** |
| Security scan | **Murah** |
| Update WORKING_MEMORY.md | **Murah** |

---

## Rules yang Tidak Boleh Dilanggar

```
❌ AI murah TIDAK BOLEH membuat keputusan arsitektur
❌ AI murah TIDAK BOLEH menambah dependency baru tanpa persetujuan
❌ AI murah TIDAK BOLEH skip pre-flight check
❌ AI TIDAK BOLEH lanjut jika ada temuan CRITICAL dari security scan
❌ AI TIDAK BOLEH merge jika unit test belum hijau
✅ Semua keputusan arsitektur HARUS masuk ke DECISIONS.md
✅ WORKING_MEMORY.md HARUS diupdate di setiap akhir sesi
✅ Review rubrik HARUS diikuti konsisten setiap kali review
```
