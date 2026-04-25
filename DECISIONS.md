# Architecture Decision Records (ADR)
> Semua keputusan arsitektur yang sudah disepakati dicatat di sini.
> 
> ⚠️ **PERINGATAN UNTUK AI AGENT:**  
> File ini adalah kontrak arsitektur. Kamu DILARANG menyimpang dari keputusan 
> yang statusnya **Accepted** tanpa mendapat persetujuan eksplisit dari developer.
> Jika kamu menemukan konflik antara task dan ADR di sini, HENTIKAN dan laporkan.

---

## Cara Membaca File Ini

Setiap ADR punya status:
- **Accepted** → Keputusan berlaku, WAJIB diikuti
- **Deprecated** → Pernah berlaku, sudah digantikan ADR lain (lihat "Superseded by")
- **Proposed** → Masih dalam diskusi, JANGAN diimplementasi dulu
- **Rejected** → Pernah diusulkan tapi ditolak — catat alasannya agar tidak diusulkan ulang

---

## Index

| ADR ID | Judul | Status | Tanggal |
|---|---|---|---|
| ADR-001 | [Contoh: Arsitektur Package Struktur] | Accepted | YYYY-MM-DD |
| ADR-002 | [Contoh: Exception Handling Strategy] | Accepted | YYYY-MM-DD |
| ADR-003 | [Contoh: Autentikasi menggunakan JWT] | Accepted | YYYY-MM-DD |

---

## Template ADR

Gunakan template ini setiap kali ada keputusan baru:

```
---
## ADR-XXX: [Judul Keputusan]

**Status:** Proposed | Accepted | Deprecated | Rejected  
**Tanggal:** YYYY-MM-DD  
**Diputuskan oleh:** Developer + AI [nama model]  
**Issue terkait:** ISSUE-XXX  
**Superseded by:** ADR-YYY (isi jika status Deprecated)

### Konteks
[Mengapa keputusan ini perlu dibuat? Apa situasi atau masalah yang memaksa 
kita harus memilih? Jelaskan trade-off yang dihadapi.]

### Opsi yang Dipertimbangkan
1. **[Opsi A]** — [kelebihan dan kekurangan]
2. **[Opsi B]** — [kelebihan dan kekurangan]
3. **[Opsi C]** — [kelebihan dan kekurangan]

### Keputusan
[Opsi mana yang dipilih dan MENGAPA. Harus spesifik dan actionable.]

### Konsekuensi
**Positif:**
- [Dampak baik dari keputusan ini]

**Negatif / Trade-off:**
- [Apa yang kita korbankan atau terima sebagai konsekuensi]

### Implementasi
[Bagaimana keputusan ini diterapkan ke codebase — boleh berisi contoh kode 
singkat, nama package, atau naming convention yang disepakati]

---
```

---

## ADR-001: [Contoh — Struktur Package]

**Status:** Accepted  
**Tanggal:** YYYY-MM-DD  
**Diputuskan oleh:** Developer  
**Issue terkait:** -  
**Superseded by:** -

### Konteks
Perlu konsistensi struktur package agar AI agent tidak membuat package 
di lokasi yang berbeda-beda setiap mengerjakan fitur baru.

### Opsi yang Dipertimbangkan
1. **Package by layer** (controller, service, repository) — familiar tapi susah scale
2. **Package by feature** (user, order, payment) — lebih modular
3. **Hybrid: feature di dalam layer** — trade-off keduanya

### Keputusan
Gunakan **package by feature** dengan struktur:

```
com.example.appname
├── [feature]/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── domain/        ← entity, value object
│   ├── dto/           ← request/response DTO
│   └── exception/     ← feature-specific exception
├── common/
│   ├── config/
│   ├── exception/     ← global exception handler
│   ├── security/
│   └── util/
```

### Konsekuensi
**Positif:**
- Setiap fitur self-contained, mudah dihapus atau di-extract jadi microservice
- AI agent tidak perlu tebak-tebak lokasi file

**Negatif / Trade-off:**
- Sedikit lebih verbose untuk fitur kecil
- Butuh disiplin untuk tidak cross-import antar feature secara sembarangan

### Implementasi
Setiap kali membuat fitur baru, buat package `com.example.appname.[feature-name]/`
terlebih dahulu sebelum membuat class apapun.

---

## ADR-002: [Contoh — Exception Handling Strategy]

**Status:** Accepted  
**Tanggal:** YYYY-MM-DD  
**Diputuskan oleh:** Developer  
**Issue terkait:** -

### Konteks
Perlu strategi yang konsisten untuk exception handling agar response error
dari API selalu dalam format yang sama, mudah dikonsumsi oleh frontend/client.

### Keputusan
Gunakan **centralized exception handling** dengan `@RestControllerAdvice`:

```java
// Pattern response error yang disepakati:
{
  "timestamp": "ISO-8601",
  "status": 400,
  "error": "VALIDATION_ERROR",
  "message": "pesan yang human-readable",
  "path": "/api/v1/...",
  "details": [] // opsional, untuk validation error per field
}
```

Exception hierarchy:
- `BaseException` (abstract) — semua custom exception extend ini
- `ResourceNotFoundException` → HTTP 404
- `ValidationException` → HTTP 400
- `BusinessRuleException` → HTTP 422
- `UnauthorizedException` → HTTP 401

### Konsekuensi
**Positif:**
- Response error konsisten di semua endpoint
- AI agent tahu persis exception mana yang harus dilempar

**Negatif / Trade-off:**
- Perlu disiplin untuk tidak throw generic `RuntimeException` langsung

### Implementasi
Semua exception custom taruh di `common/exception/` untuk yang global,
atau `[feature]/exception/` untuk yang feature-specific.
Jangan pernah return error langsung dari controller — selalu lewat exception.

---

## ADR-003: [Contoh — API Versioning]

**Status:** Accepted  
**Tanggal:** YYYY-MM-DD  
**Diputuskan oleh:** Developer  
**Issue terkait:** -

### Konteks
Perlu strategi versioning API yang konsisten sejak awal agar tidak breaking change
di masa depan ketika ada client yang sudah consume API ini.

### Keputusan
Gunakan **URL path versioning**: `/api/v1/[resource]`

Aturan:
- Semua endpoint wajib punya prefix `/api/v{N}/`
- Major version naik hanya ketika ada breaking change
- Endpoint lama dipertahankan minimal 2 sprint setelah versi baru rilis

### Konsekuensi
**Positif:**
- Mudah dibaca, mudah di-route di API gateway
- Eksplisit dan tidak ambigu

**Negatif / Trade-off:**
- URL sedikit lebih panjang
- Perlu update semua test ketika ada versi baru

---

*Tambahkan ADR baru di bawah ini mengikuti template di atas.*
