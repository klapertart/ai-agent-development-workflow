# WORKING MEMORY
> File ini adalah "memori jangka pendek" dari AI Agent.
> WAJIB dibaca di awal setiap sesi. WAJIB diupdate di akhir setiap sesi.
> Jika AI murah tidak update file ini di akhir sesi, anggap sesi tidak selesai.

---

## 🗓️ Last Updated
- **Tanggal:** YYYY-MM-DD
- **Sesi ke:** N
- **Dikerjakan oleh:** AI Model [nama model]

---

## 🎯 Active Issue
- **Issue ID:** ISSUE-XXX
- **Judul:** [nama issue yang sedang dikerjakan]
- **Implementation Plan:** `docs/implementation/IMPL_ISSUE-XXX.md`
- **Status Issue:** In Progress | Review | Fixing | Testing | Done

---

## ✅ Task Progress

| Task ID | Nama Task | Status | Catatan |
|---|---|---|---|
| IMPL-XXX-T1 | [nama task] | ✅ Done | |
| IMPL-XXX-T2 | [nama task] | 🔄 In Progress | Baru sampai method X |
| IMPL-XXX-T3 | [nama task] | ⏳ Pending | Tunggu T2 selesai |
| IMPL-XXX-T4 | [nama task] | ⏳ Pending | |

**Task aktif sesi ini:** IMPL-XXX-T2  
**Task berikutnya:** IMPL-XXX-T3

---

## 📁 Files Modified This Session

```
src/main/java/com/example/[package]/
├── domain/
│   └── [ClassName].java          → [Created | Modified] — [alasan singkat]
├── service/
│   └── [ServiceName].java        → [Created | Modified] — [alasan singkat]
├── controller/
│   └── [ControllerName].java     → [Created | Modified] — [alasan singkat]
└── repository/
    └── [RepositoryName].java     → [Created | Modified] — [alasan singkat]

src/test/java/com/example/[package]/
└── service/
    └── [ServiceName]Test.java    → [Created | Modified] — [alasan singkat]
```

---

## 🧠 Decisions Made This Session

> Keputusan kecil yang dibuat selama sesi ini yang tidak sampai level ADR.
> Keputusan besar yang berdampak ke arsitektur harus masuk ke DECISIONS.md.

- **[Keputusan 1]:** [Apa yang diputuskan dan mengapa] — *Tidak perlu masuk DECISIONS.md*
- **[Keputusan 2]:** [Apa yang diputuskan dan mengapa] — *→ Sudah ditambahkan ke DECISIONS.md sebagai ADR-XXX*

---

## ⚠️ Blockers & Open Questions

> Item di sini HARUS dikonfirmasi oleh developer sebelum sesi berikutnya dilanjutkan.

- [ ] **[BLOCKER-1]:** [Deskripsi blocker] — *Butuh keputusan: [pertanyaan spesifiknya]*
- [ ] **[QUESTION-1]:** [Pertanyaan yang muncul selama coding]

---

## 📝 TODO / Tech Debt Ditemukan

> Item yang ditemukan selama implementasi tapi di luar scope task saat ini.
> Jangan dikerjakan sekarang — catat di sini dan buat issue baru jika perlu.

- **TODO-1:** `[path/ke/file.java:baris]` — [deskripsi tech debt]
- **TODO-2:** `[path/ke/file.java:baris]` — [deskripsi tech debt]

---

## 🧪 Test Status

| Jenis Test | Status | Perintah Terakhir | Hasil |
|---|---|---|---|
| Unit Test | ✅ Pass / ❌ Fail / ⏳ Belum dijalankan | `mvn test` | X passed, Y failed |
| Integration Test | ⏳ Belum dijalankan | `mvn verify` | - |
| Coverage | ⏳ Belum dicek | `mvn jacoco:report` | - |

---

## 🔒 Security Scan Status

| Tool | Status | Temuan |
|---|---|---|
| GitLeaks | ⏳ Pending | - |
| Semgrep | ⏳ Pending | - |
| Trivy | ⏳ Pending | - |
| SonarQube | ⏳ Pending | - |

---

## 📋 Next Session Checklist

> Salin dan centang ini di awal sesi berikutnya.

- [ ] Baca AGENTS.md
- [ ] Baca DECISIONS.md
- [ ] Baca WORKING_MEMORY.md (file ini)
- [ ] Baca `docs/implementation/IMPL_[ISSUE-ID].md`
- [ ] Konfirmasi task berikutnya: **IMPL-XXX-T[N]**
- [ ] Resolve blocker yang ada sebelum mulai coding

---

## 📌 Context Snapshot

> Ringkasan singkat state project untuk referensi cepat.

```
Branch aktif : feature/[nama-branch]
Base branch  : develop
Spring Boot  : [versi]
Java         : [versi]
Last commit  : [pesan commit terakhir]
```

---

*Template ini adalah bagian dari AI_WORKFLOW.md — lihat file tersebut untuk panduan lengkap workflow.*
