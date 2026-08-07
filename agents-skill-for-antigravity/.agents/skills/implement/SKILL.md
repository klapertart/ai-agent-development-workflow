---
name: implement
description: Gunakan skill ini ketika user ingin mengeksekusi implementasi kode untuk sebuah task dari implementation plan, menulis kode Spring Boot, atau mengerjakan task yang sudah didefinisikan di IMPL plan. Trigger: "implement", "kerjakan", "coding sekarang", "eksekusi task", "tulis kodenya".
---

# Implementation Skill — Tahap 4 (AI Murah)

## Goal
Mengeksekusi satu atomic task dari implementation plan secara ketat, sesuai rules yang berlaku.

## Instructions

1. **Pastikan preflight check sudah dilakukan** di sesi ini. Jika belum, jalankan skill `preflight` terlebih dahulu.

2. **Baca task definition** dari `docs/implementation/IMPL_<ISSUE-ID>.md` — kerjakan hanya task yang diminta, tidak lebih.

3. **Coding constraints WAJIB (dari AGENTS.md Cakra Workflow Service):**
   - ✅ Gunakan Constructor Injection (`@RequiredArgsConstructor`), bukan `@Autowired`
   - ✅ Gunakan `ApiResponse<T>` untuk semua response wrapper
   - ✅ Gunakan `cakra-common-lib` untuk logging, exception handling, response wrapper
   - ✅ Gunakan Lombok dan MapStruct — jangan buat boilerplate manual
   - ✅ Ikuti naming convention existing (Impl, Dto, Mapper, dll)
   - ❌ DILARANG expose JPA Entity ke Controller
   - ❌ DILARANG hardcode config — gunakan `@Value` atau `@ConfigurationProperties`
   - ❌ DILARANG menambah dependency baru tanpa persetujuan user
   - ❌ DILARANG mengubah `-prod.properties`

4. **Jika menemukan sesuatu di luar scope task:**
   - Jangan putuskan sendiri
   - Tulis sebagai `// TODO: [deskripsi] — perlu konfirmasi user`
   - Laporkan ke user setelah selesai

5. **Setelah selesai, laporkan:**
   ```
   ✅ File yang dibuat/diubah:
   - path/File.java — [alasan]

   ✅ Definisi "done" terpenuhi: [Ya/Tidak — penjelasan]

   ⚠️ TODO yang perlu review: [list atau "Tidak ada"]
   ```

## Constraints
- Scope ketat — hanya kerjakan yang ada di definisi task
- Jangan rewrite global — atomic changes only
- Jangan override DECISIONS.md
