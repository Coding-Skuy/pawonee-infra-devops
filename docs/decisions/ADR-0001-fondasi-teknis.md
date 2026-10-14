# ADR-0001 — Fondasi teknis pawonee-infra-devops

Status: Diterima | Tanggal: 2026-10-14

## Konteks

Infra mengorkestrasi seluruh repo Pawonee dan menegakkan QA gate bersama. Sumber kebenaran: Pawonee-TownHall `platform/versions/v1.0.0` (https://github.com/Coding-Skuy/Pawonee-TownHall/tree/main/platform/versions/v1.0.0), beku 2026-10-07; lintas divisi: [KONTEKS-LINTAS-DIVISI.md](https://github.com/Coding-Skuy/ChefGenie-TownHall/blob/main/KONTEKS-LINTAS-DIVISI.md). Tiket fondasi: PWN-OPS-001 (issue #1).

## Keputusan

1. CI memblokir rilis bila `cargo test`, uji web, build APK KMP, atau evaluasi bank 12/12 gagal.
2. Deploy per env lewat compose (lokal) dan k8s; Postgres 16.x dengan DB logis `pawonee`; satu DB per service.
3. Secrets per env (`DATABASE_URL`, kunci JWT audiens `pawonee`, service key, API key perangkat); service key least-privilege dan dirotasi 90 hari.
4. Rilis mobile lewat Play Store internal, APK sideload, dan TestFlight; tidak ada target Windows untuk Pawonee.
5. Runbook deploy dan rollback wajib sebelum produksi; `VERSIONS.md` diselaraskan setelah migrasi web ke Bun + SvelteKit.

## Konsekuensi

- `VERSIONS.md` (Node + Next.js) harus diperbarui setelah migrasi `pawonee-web`.
- Pipeline CI menjadi satu-satunya jalur rilis; commit langsung tanpa CI hijau dilarang.
- Rotasi secret menjadi tugas operasional terjadwal.
