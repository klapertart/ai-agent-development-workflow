# Security Scan Implementation Plan — Spring Boot

## Context

- OS: Windows PowerShell
- Service: Spring Boot (Maven)
- Scan order: Gitleaks → SonarQube → Trivy
- Executor: AI model (Gemini Flash atau sejenisnya)

---

## Prerequisites

Pastikan tools berikut denga versi minimal sudah terinstall sebelum menjalankan plan ini:

- Git
- Java 17+
- Maven 3.6+
- Docker (untuk SonarQube)
- Gitleaks (via `winget install gitleaks`)
- Trivy (via `winget install AquaSecurity.Trivy`)

---

## Variables

```powershell
# Jalankan ini di terminal sebelum menjalankan step-step berikutnya
$PROJECT_DIR     = "D:\Workspace\Tritronik\Cakra\code\workdir\orchestrator\cakra-workflow-service"
$SONAR_URL       = "http://localhost:9001"
$SONAR_TOKEN     = "squ_e020eae5a7f18ce6737504aa885ea8c46d9d6e04"
$SONAR_PROJECT_KEY  = "cakra-workflow-service"
$SONAR_PROJECT_NAME = "Cakra Workflow Service"
$REPORT_DIR      = "D:\Workspace\Tritronik\Cakra\code\workdir\orchestrator\cakra-workflow-service\scan-reports"
```

---

## Step 1 — Gitleaks Scan

**Tujuan:** Memastikan tidak ada credential, API key, atau secret yang bocor di repository.

**Jalankan perintah berikut di PowerShell dari dalam folder project:**

```powershell
# Pastikan folder report ada
New-Item -ItemType Directory -Force -Path $REPORT_DIR

gitleaks detect --source . -v `
  --report-format json `
  --report-path "$REPORT_DIR\gitleaks-report.json"
```

**Kriteria lanjut ke step berikutnya:**
- Perintah selesai tanpa error
- Output menunjukkan `leaks found: 0`
- File `gitleaks-report.json` terbuat di `$REPORT_DIR`

**Jika ada temuan:**
- Baca file `gitleaks-report.json`
- Identifikasi file dan baris yang bermasalah
- Hapus atau pindahkan credential ke environment variable
- Jalankan ulang scan sampai tidak ada temuan
- Jangan lanjut ke step berikutnya sebelum bersih

---

## Step 2 — SonarQube Scan

**Tujuan:** Analisis kualitas kode dan keamanan statis (SAST) — mendeteksi bug, code smell, dan celah keamanan di source code.

### Step 2a — Pastikan SonarQube Berjalan

```powershell
curl.exe -s http://localhost:9001/api/system/status
```

Response yang diharapkan:
```json
{"status":"UP"}
```

*Jika SonarQube belum UP, tunggu 60 detik lalu cek kembali status.*

### Step 2b — Jalankan Scan

```powershell
mvn clean verify sonar:sonar `
  "-Dsonar.host.url=$SONAR_URL" `
  "-Dsonar.projectKey=$SONAR_PROJECT_KEY" `
  "-Dsonar.projectName=$SONAR_PROJECT_NAME" `
  "-Dsonar.token=$SONAR_TOKEN"
```

**Kriteria lanjut ke step berikutnya:**
- Output Maven menunjukkan `BUILD SUCCESS`
- Output menunjukkan `ANALYSIS SUCCESSFUL`
- Dashboard SonarQube bisa diakses di `$SONAR_URL/dashboard?id=$SONAR_PROJECT_KEY`

### Step 2c — Export Hasil Scan

```powershell
curl.exe -s `
  -u "${SONAR_TOKEN}:" `
  "${SONAR_URL}/api/issues/search?componentKeys=${SONAR_PROJECT_KEY}&statuses=OPEN&ps=500" `
  -o "$REPORT_DIR\sonar-report.json"

Write-Host "SonarQube report saved to $REPORT_DIR\sonar-report.json"
```

**Jika ada temuan (Semua Severity - Blocker hingga Low):**
- Baca file `sonar-report.json`
- Identifikasi isu berdasarkan severity dan tipe (Bug, Vulnerability, Code Smell)
- Perbaiki semua isu mulai dari BLOCKER, CRITICAL, MAJOR, hingga severity MEDIUM dan LOW
- **Khusus Duplications:** Pastikan tingkat duplikasi kode di bawah 3% (**Duplications < 3%**).
- **Khusus Coverage:** Pastikan cakupan kode baru (**New Coverage**) mencapai minimal **80%**.
- **Khusus Maintainability:** Pastikan semua Code Smells diperbaiki untuk mencapai Rating Maintainability: A
- **Unused Imports:** Bersihkan semua sisa package import yang tidak digunakan (Rule java:S1128).
- Jalankan ulang scan untuk verifikasi

### Step 2d — Export Unused Imports (Opsional/Spesifik)

Jika ingin fokus membersihkan import saja, jalankan:

```powershell
curl.exe -s `
  -u "${SONAR_TOKEN}:" `
  "${SONAR_URL}/api/issues/search?componentKeys=${SONAR_PROJECT_KEY}&rules=java:S1128&statuses=OPEN&ps=500" `
  -o "$REPORT_DIR\unused-imports.json"

Write-Host "Unused imports list saved to $REPORT_DIR\unused-imports.json"
```

---

## Step 3 — Trivy Scan

**Tujuan:** Mendeteksi celah keamanan (CVE) pada dependency/library yang digunakan di `pom.xml`.

### Step 3a — Jalankan Scan

```powershell
# Scan Filesystem dengan ketelitian tinggi (vuln, secret, config)
trivy fs . `
  --scanners vuln,secret,config `
  --format json `
  --output "$REPORT_DIR\trivy-report.json"
```

### Step 3b — Tampilkan Ringkasan di Terminal

```powershell
trivy fs . --severity CRITICAL,HIGH,MEDIUM,LOW
```

**Kriteria selesai:**
- Perintah selesai tanpa error
- File `trivy-report.json` terbuat di `$REPORT_DIR`

- Jika `Fixed Version` kosong, catat sebagai known issue dan pantau terus

---

## Step 4 — Final Audit Summary (AI Consolidation)

**Tujuan:** Mengkonsolidasikan semua temuan ke dalam satu laporan ringkas untuk review developer.

**AI Executor harus membuat file `docs/review/SECURITY_AUDIT_REPORT.md` dengan format:**

1. **Executive Summary**: Status PASS/FAIL untuk setiap tool.
2. **Issue Table**: Rekapitulasi temuan HIGH/CRITICAL (jika ada).
3. **Metric Table**: Duplications, Coverage, dan Code Smells.
4. **Recommendation**: Langkah mitigasi jika masih ada temuan LOW/MEDIUM.

---

## Output Files

Setelah semua step selesai, folder `$REPORT_DIR` akan berisi:

```
scan-reports/
├── gitleaks-report.json    ← hasil scan secret
├── sonar-report.json       ← hasil scan SAST (all issues)
├── unused-imports.json     ← daftar sisa import tidak terpakai
└── trivy-report.json       ← hasil scan dependency CVE
```

---

## Completion Criteria

Semua step dianggap selesai jika:

- [ ] Gitleaks: `leaks found: 0`
- [ ] SonarQube: `BUILD SUCCESS` dan `Quality Gate: PASSED`
- [ ] SonarQube Metrics: `Duplications < 3%`, `New Coverage >= 80%`, `Rating Maintainability: A`
- [ ] Trivy: `0 Vulnerabilities` (CRITICAL, HIGH, MEDIUM, LOW)
- [ ] Audit Report: `SECURITY_AUDIT_REPORT.md` telah dibuat

---

## Notes for AI Executor

- Jalankan setiap step secara berurutan. Jangan lanjut ke step berikutnya jika step sebelumnya gagal.
- Jika ada perintah yang gagal, tampilkan pesan error lengkapnya dan hentikan eksekusi.
- Semua perintah dijalankan di Windows PowerShell.
- Variabel `$REPORT_DIR` harus sudah ada sebagai folder sebelum menjalankan perintah.
- Jangan modifikasi source code tanpa konfirmasi dari developer.
- Laporkan ringkasan hasil setiap step sebelum melanjutkan ke step berikutnya.
