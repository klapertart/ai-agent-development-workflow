---
name: adr
description: Gunakan skill ini ketika ada keputusan arsitektur baru yang perlu dicatat, user menyetujui pendekatan teknis tertentu, atau ada keputusan tentang pattern, library, atau struktur yang mengikat seluruh tim. Trigger: "catat keputusan", "ADR", "DECISIONS.md", "keputusan arsitektur", "sepakat pakai".
---

# Architecture Decision Record Skill — Tahap 2 (AI Pintar)

## Goal
Mencatat keputusan arsitektur ke `DECISIONS.md` dalam format ADR standar agar mengikat semua AI yang bekerja di project ini.

## Instructions

1. **Tambahkan entry baru ke `DECISIONS.md`** dengan format ADR:

```
## ADR-[N] — [Nama Keputusan]

**Tanggal:** [tanggal hari ini]
**Status:** Accepted

### Konteks
[Mengapa keputusan ini perlu dibuat — situasi atau masalah yang mendorong keputusan]

### Keputusan
[Apa yang diputuskan secara eksplisit]

### Konsekuensi
**Positif:**
- [Dampak baik dari keputusan ini]

**Negatif / Trade-off:**
- [Dampak yang perlu diwaspadai]

### Berlaku untuk
- [Modul / service / scope yang terdampak]
```

2. **Tegaskan ke user** bahwa keputusan ini mengikat — AI murah yang mengerjakan implementasi tidak boleh menyimpang tanpa persetujuan eksplisit.

## Constraints
- ADR-ID harus sequential (cek DECISIONS.md yang ada)
- Jangan mendokumentasikan keputusan yang belum disetujui user
- Keputusan yang sudah Accepted tidak boleh diubah oleh AI — harus lewat user
