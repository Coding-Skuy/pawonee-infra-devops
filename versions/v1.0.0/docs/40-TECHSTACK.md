> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# 40-TECHSTACK — Tumpukan Teknologi (pawonee-infra-devops)

Mengacu: Pawonee-TownHall `platform/versions/v1.0.0` (https://github.com/Coding-Skuy/Pawonee-TownHall/tree/main/platform/versions/v1.0.0), beku 2026-10-07. Tidak menduplikasi BRD/PRD/FRD/FSD/SRS; hanya mengunci stack repo ini.

## 1. Matriks stack

| Komponen | Kondisi saat ini (tercatat) | Target terkunci v1.0.0 |
|---|---|---|
| Basis data | postgres:16.4 (compose) | Postgres 16.x, DB `pawonee` |
| Backend build | Rust 1.82.0 | Rust stabil terbaru (mengikuti backend) |
| Web build | Node.js 22.12.0 + Next.js 15.1.6 | Bun (mengikuti migrasi web) |
| KMP build | Kotlin 2.0.21 / Compose 1.7.0 | Kotlin 2.2.20 / Compose 1.8.2 / Navigation3 1.0.0 |
| Python | 3.12.7 | 3.12.x |
| Orkestrasi | docker compose + `k8s/` | compose (lokal) + k8s (env) |
| CI | `.github/workflows/ci.yml` | QA gate: cargo test, bun, APK KMP, eval 12/12 |

## 2. Drift yang tercatat

`VERSIONS.md` dan README masih memuat Node.js 22.12.0 + Next.js 15.1.6 (drift web); diselaraskan setelah tiket `PWN-WEB-005` selesai.

## Batasan

- Dokumen ini tidak mengubah manifes atau kode; kondisi saat ini dicatat apa adanya dari manifes/README repo.
- Versi dikunci eksak di manifes saat kickoff coding; perubahan versi dicatat di dokumen ini lewat versi dokumen berikutnya dan ADR baru.
- JWT audiens `pawonee`, DB logis `pawonee`, dan `:shared:pantry-resep` milik Pawonee tidak berubah oleh dokumen ini.
