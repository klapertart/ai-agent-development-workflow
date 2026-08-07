---
name: unit-test
description: Gunakan skill ini ketika user ingin membuat unit test untuk kode yang baru diimplementasi, memastikan test coverage, atau menulis test dengan JUnit dan Mockito. Trigger: "buat unit test", "tulis test", "unit test", "test coverage", "JUnit", "Mockito".
---

# Unit Test Skill — Tahap 7 (AI Murah)

## Goal
Membuat unit test yang memadai untuk semua kode yang diimplementasi, memenuhi coverage threshold yang ditetapkan.

## Instructions

1. **Identifikasi kode yang perlu ditest:**
   - Baca `docs/implementation/IMPL_<ISSUE-ID>.md` untuk tahu class/method yang dibuat
   - Fokus pada public methods dan business logic

2. **Stack testing wajib (sesuai AGENTS.md):**
   - JUnit 5
   - Mockito untuk mocking dependency
   - AssertJ untuk assertions
   - `@SpringBootTest` hanya jika benar-benar diperlukan — prefer pure unit test

3. **Requirement tiap test:**
   - Minimal 1 happy path test per public method
   - Cover semua branch kondisi (if/else, switch)
   - Cover edge case yang didefinisikan di `ISSUES.md`
   - Nama test deskriptif: `should_[expected]_when_[condition]`
   - Gunakan `@DisplayName` untuk readability

4. **Coverage target:**
   - Line coverage: minimal 80%
   - Branch coverage: minimal 70%

5. **Setelah test ditulis:**
   - Jalankan: `mvn test`
   - Laporkan output — pastikan semua test hijau
   - Jika ada yang fail, fix sebelum lanjut

6. Informasikan ke user untuk lanjut ke skill `coverage` setelah test hijau.

## Constraints
- Jangan test implementasi detail — test behavior
- Mock semua dependency eksternal (DB, HTTP, Kafka, dll)
- Jangan gunakan `@SpringBootTest` jika bisa ditest tanpa konteks Spring
