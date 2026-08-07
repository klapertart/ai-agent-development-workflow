---
description: Generate unit test untuk kode yang baru diimplementasi. Jalankan setelah review approved.
---

Jalankan skill `unit-test` untuk task: $ARGUMENTS

Stack: JUnit 5, Mockito, AssertJ. Hindari @SpringBootTest jika bisa pure unit test.
Nama test: `should_[expected]_when_[condition]`. Gunakan `@DisplayName`.
Coverage target: line ≥ 80%, branch ≥ 70%.

Setelah test ditulis, jalankan `mvn test` dan pastikan semua hijau.
Laporkan output hasil test.
