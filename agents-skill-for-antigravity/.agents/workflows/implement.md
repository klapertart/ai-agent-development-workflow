---
description: Implementasi kode untuk satu atomic task dari implementation plan. Jalankan setelah /preflight dikonfirmasi.
---

Jalankan skill `implement` untuk task: $ARGUMENTS

Baca `docs/implementation/IMPL_$ARGUMENTS.md` dan kerjakan task yang diminta.

Constraints wajib dari `AGENTS.md`:
- Constructor Injection (`@RequiredArgsConstructor`), bukan `@Autowired`
- Gunakan `ApiResponse<T>` untuk response wrapper
- Gunakan `cakra-common-lib` untuk logging, exception handling, response wrapper
- DILARANG expose JPA Entity ke Controller
- DILARANG hardcode config
- DILARANG tambah dependency baru tanpa persetujuan user
- DILARANG ubah `-prod.properties`

Scope ketat — hanya kerjakan yang ada di definisi task. Tulis TODO untuk hal di luar scope.
