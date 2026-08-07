---
description: Cek test coverage dengan JaCoCo. Jalankan setelah /unit-test selesai.
---

Jalankan skill `coverage` untuk module: $ARGUMENTS

Eksekusi: `mvn jacoco:report -pl $ARGUMENTS`
Laporkan: overall line coverage, branch coverage, dan class yang di bawah threshold.
Threshold wajib: line ≥ 80%, branch ≥ 70%.
Jangan lanjut ke integration test sebelum threshold terpenuhi.
