# Antigravity Agent Skills 🚀

Folder ini isinya kumpulan *skill* kustom (yang biasa dipanggil lewat *slash command*) buat AI Agent **Antigravity**. Tujuannya simpel: biar kerjaan coding dan *workflow development* bisa lebih otomatis dan rapi tanpa harus capek ngetik prompt panjang-panjang terus.

## 🤔 Apa itu Slash Command (Skill)?

Intinya, **Slash command** atau **Skill** itu semacam *shortcut* pintar yang nempel langsung di dalam chat UI Antigravity. 

Daripada pegel ngetik instruksi yang sama berulang-ulang buat tugas rutin (kayak minta AI nge-review kode, bikin *implementation plan*, atau nulis *unit test*), tinggal panggil aja *skill* yang udah disiapin. Cukup ketik garis miring (misal: `/impl-plan`), dan si Agent bakal langsung ngerti harus ngerjain apa, format outputnya kayak gimana, dan pantangannya apa aja. Jauh lebih praktis!

## 🛠️ Cara Pasang & Pakenya Gimana?

Biar Antigravity bisa ngenalin dan ngejalanin *skill* ini di project, ikutin 3 langkah gampang ini:

1. **Copy ke Root Project**  
   Copy aja folder `.agents` yang ada di sini, terus paste di folder paling luar (*root folder*) dari project atau workspace yang lagi dikerjain.
   
2. **Auto-Kedetect Kok!**  
   Nggak perlu ribet setting macem-macem atau restart IDE. Antigravity udah punya fitur *auto-discovery* yang bakal nge-scan folder `.agents` di workspace secara otomatis. Begitu di-copy, daftarnya langsung ke-update dan siap dipake!

3. **Tinggal Panggil di Chat**  
   Buka tab chat Antigravity, terus ketik tanda garis miring (`/`). Nanti bakal muncul *dropdown* berisi semua *slash command* yang tersedia. Atau bisa juga langsung ketik nama command-nya (contoh: `/review`, `/unit-test`). Agent siap ngerjain tugas yang diminta!

## 📂 Struktur Foldernya

Di dalam folder `.agents`, ada dua sub-folder utama dengan fungsinya masing-masing:

### 1. Folder `skills`
Di sini tiap *skill* disimpen di foldernya masing-masing (path: `.agents/skills/`). File paling penting di setiap folder *skill* itu namanya `SKILL.md`. Isinya kebagi dua:
- **Frontmatter (YAML):** Cuma buat nentuin nama *slash command* dan sedikit deskripsi singkatnya.
- **Isi Instruksi (Markdown):** Ini nih otaknya! Isinya panduan detail, aturan (constraints), dan referensi spesifik tentang **BAGAIMANA** agent harus mengeksekusi tugas tersebut.

### 2. Folder `workflows`
File-file di dalam folder ini (misal: `impl-plan.md`, `review.md`) berfungsi sebagai **skrip eksekusi / makro** (SOP terstruktur). 
Kalau `skills` itu ngasih tau *cara* ngerjainnya, nah `workflows` ini ngasih tau **APA** yang harus dilakuin pertama kali saat perintah dipanggil. Contohnya, ngatur urutan biar Agent diwajibkan buat baca *context* penting (kayak `AGENTS.md` atau catatan *issue*) lebih dulu, sebelum lanjut mengeksekusi *skill* utamanya. Intinya biar proses kerjanya nggak ngasal dan tetep tertib.

## 📄 File Konfigurasi Tambahan (Penting!)

Selain folder `.agents`, di repositori ini juga ada 2 file tambahan yang **WAJIB ikut di-copy ke Root Project** (ditaruh sejajar/satu level dengan folder `.agents`). Namun perlu dicatat, file-file ini butuh sedikit *adjustment* sebelum dipakai:

### 1. `AGENTS.md`
File ini bertindak sebagai **Ground Truth / AI Governance Layer**. Isinya adalah aturan main utama (*global rules*) yang wajib dipatuhi si Agent selama ngerjain project, seperti *tech stack* yang dipake, *coding conventions*, dan standar keamanannya.

**⚠️ Bagian yang Perlu Di-adjust:**
Template bawaan ini disetting khusus buat **project Spring Boot (Java)**. Kalau mau dipakai buat bahasa/framework lain, jangan lupa ganti bagian ini:
- **Section "Project Context" & "Tech Stack":** Sesuaikan bahasa dan framework-nya (misal diganti jadi Node.js, React, atau Go).
- **Section "Strict Coding Conventions":** Sesuaikan standar penulisan kode, manajemen *dependency*, dan *best practices* dari ekosistem yang sesuai (soalnya kalau pakai aturan Spring Boot kayak larangan `@Autowired` pasti nggak nyambung di project jenis lain).

### 2. `SCAN-IMPLEMENTATION-PLAN.md`
File ini adalah contekan (panduan *step-by-step*) buat Agent saat diminta ngejalanin *security scan* (seperti Gitleaks, SonarQube, dan Trivy).

**⚠️ Bagian yang Perlu Di-adjust:**
Sama halnya, file ini **cuma *sample* panduan buat project Spring Boot (Maven)**. Wajib sesuaikan bagian-bagian ini sebelum dipakai:
- **Prerequisites:** Ganti persyaratannya (seperti Java & Maven) ke *runtime* atau *package manager* yang dipakai project aslinya.
- **Variables:** Bagian ini wajib diganti! Sesuaikan value `$PROJECT_DIR`, credential seperti `$SONAR_TOKEN`, sampai ke nama/key project `$SONAR_PROJECT_KEY` pakai data asli.
- **Perintah Eksekusi / Command:** Di sample ini cara *trigger* scan SonarQube pakai perintah dari Maven (`mvn sonar:sonar`). Untuk project selain Java/Maven, ini harus diubah jadi *command* yang relevan (misalnya pakai `sonar-scanner` murni atau lewat script npm).

### 3. `DECISIONS.md`
File ini adalah tempat buat nyatet semua keputusan arsitektur atau *Architecture Decision Records* (ADR) yang udah disepakati di project.
Karena instruksi di dalam beberapa *skill* mewajibkan Agent untuk baca file ini buat tau konteks sebelum nulis kode, **sangat disarankan** buat bikin file kosong bernama `DECISIONS.md` di Root Project (kalau belum ada). Biar pas Agent jalan, dia punya acuan dan nggak ngelanggar keputusan arsitektur yang udah dibuat.
