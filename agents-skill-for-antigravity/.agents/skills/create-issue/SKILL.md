---
name: create-issue
description: Gunakan skill ini ketika user ingin mencatat hasil diskusi planning ke ISSUES.md, membuat issue baru, mendokumentasikan fitur atau bug yang sudah didiskusikan. Trigger: "tuangkan ke issue", "buat issue", "catat ke ISSUES.md", "dokumentasikan".
---

# Create Issue Skill — Tahap 1 (AI Pintar)

## Goal
Menuangkan hasil diskusi planning ke dalam format standar `ISSUES.md`.

## Instructions

1. **Pastikan diskusi planning sudah selesai** — jika belum, arahkan ke skill `planning` terlebih dahulu.

2. **Tulis entry baru di `ISSUES.md`** dengan format berikut:

```
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
```

## Constraints
- Acceptance criteria harus bisa diverifikasi dengan unit test
- Jangan langsung membuat implementation plan setelah issue dibuat
- ISSUE-ID harus unik dan sequential (cek ISSUES.md yang ada)
