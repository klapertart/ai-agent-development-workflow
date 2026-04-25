# Workflow Pengembangan AI Agent

Repositori ini menyediakan alur kerja (workflow) yang matang dan terstruktur untuk pengembangan perangkat lunak berbantuan AI, yang dioptimalkan khusus untuk proyek **Spring Boot**. Repositori ini mendefinisikan siklus hidup multi-tahap yang dirancang untuk memaksimalkan kekuatan berbagai model AI (misalnya, model "Pintar" untuk perencanaan dan model "Eksekusi" untuk pengkodean) sambil memastikan konsistensi arsitektur dan keamanan.

## 🚀 Fitur Utama

- **Model Tiering:** Menggunakan model dengan penalaran tinggi secara strategis untuk perencanaan dan model yang hemat biaya untuk eksekusi.
- **Integritas Arsitektur:** Mengharuskan penggunaan `DECISIONS.md` (ADR) untuk mencegah "silent drift" (pergeseran tersembunyi) dalam desain sistem.
- **Tugas Atomik:** Memecah fitur menjadi tugas-tugas kecil yang dapat diverifikasi dalam satu sesi AI.
- **Persistensi Konteks:** Menggunakan `WORKING_MEMORY.md` untuk menjembatani kesenjangan antar sesi pengembangan.
- **Keamanan Utama:** Mencakup fase pemindaian keamanan wajib dalam siklus hidup pengembangan.

## 📁 Dokumen Inti

| File | Tujuan |
|---|---|
| [**AI_WORKFLOW.md**](./AI_WORKFLOW.md) | Panduan utama untuk siklus hidup pengembangan (Perencanaan → Implementasi → Review → Test → Keamanan → Changelog). |
| [**DECISIONS.md**](./DECISIONS.md) | Architecture Decision Records (ADR) - "Sumber Kebenaran" untuk batasan teknis. |
| [**WORKING_MEMORY.md**](./WORKING_MEMORY.md) | Memori jangka pendek bagi agent untuk melacak status sesi, progres tugas, dan hambatan (blockers). |

## 🛠 Ikhtisar Workflow

Workflow ini mengikuti siklus hidup **Riset → Strategi → Eksekusi** melalui 11 tahapan:

1.  **Tahap 1-2:** Persiapan dan Pengumpulan Konteks.
2.  **Tahap 3:** **Perencanaan (AI Pintar)** - Mendefinisikan issue dan kriteria penerimaan di `ISSUES.md`.
3.  **Tahap 4:** **Rencana Implementasi (AI Pintar)** - Memecah issue menjadi tugas-tugas atomik.
4.  **Tahap 5:** **Pre-flight & Implementasi (AI Eksekusi)** - Mengkodekan tugas individu.
5.  **Tahap 6-7:** **Code Review & Perbaikan (AI Pintar/Eksekusi)** - Kontrol kualitas iteratif.
6.  **Tahap 8-9:** **Pengujian (AI Eksekusi)** - Unit Test dan Integration Test.
7.  **Tahap 10:** **Pemindaian Keamanan (AI Eksekusi)** - Pemeriksaan kerentanan otomatis.
8.  **Tahap 11:** **Changelog (AI Eksekusi)** - Mendokumentasikan perubahan.

## 🚦 Aturan untuk AI Agent

1.  **Jangan pernah abaikan `DECISIONS.md`**: Semua pilihan arsitektur bersifat mengikat.
2.  **Update `WORKING_MEMORY.md`**: Tidak ada sesi yang lengkap tanpa memperbarui status.
3.  **Tetap Atomik**: Jangan mencoba menyelesaikan beberapa masalah kompleks sekaligus dalam satu giliran.
4.  **Pre-flight Wajib**: Selalu verifikasi konteks sebelum memulai tugas.

---
*Dibuat oleh [kurakuraninja](https://github.com/kurakuraninja)*
