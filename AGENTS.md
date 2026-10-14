# AGENTS.md — pawonee-infra-devops

Repo `pawonee-infra-devops` adalah infrastruktur Pawonee: CI semua repo, deploy per env (compose + k8s), manajemen secrets, runbook, dan jalur rilis mobile.

## Sumber kebenaran

Seluruh requirement berada di `Pawonee-TownHall` (versi beku v1.0.0). Repo ini dilarang menduplikasi requirement; bila requirement berubah, buka PR di TownHall dulu.

| Dokumen | Isi yang mengikat repo ini |
|---|---|
| [`platform/versions/v1.0.0/BRD/00-ikhtisar.md`](https://github.com/Coding-Skuy/Pawonee-TownHall/blob/main/platform/versions/v1.0.0/BRD/00-ikhtisar.md) | BR-001..BR-007: bank 12 resep, stok Pedaree sebagai masukan, JWT |
| [`platform/versions/v1.0.0/SRS/30-non-functional.md`](https://github.com/Coding-Skuy/Pawonee-TownHall/blob/main/platform/versions/v1.0.0/SRS/30-non-functional.md) | Kebutuhan non-fungsional |
| [`platform/versions/v1.0.0/SRS/40-acceptance-criteria.md`](https://github.com/Coding-Skuy/Pawonee-TownHall/blob/main/platform/versions/v1.0.0/SRS/40-acceptance-criteria.md) | QA-001..QA-004 (QA gate) |
| [`platform/versions/v1.0.0/FSD/30-kontrak.md`](https://github.com/Coding-Skuy/Pawonee-TownHall/blob/main/platform/versions/v1.0.0/FSD/30-kontrak.md) | Kontrak API, event, galat, autentikasi |
| [`KONTEKS-LINTAS-DIVISI.md`](https://github.com/Coding-Skuy/ChefGenie-TownHall/blob/main/KONTEKS-LINTAS-DIVISI.md) (ChefGenie-TownHall) | Event, komoditas pilot (§4), grade (§5), satuan/ID/uang (§6), metrik (§7), isu terbuka (§8) |
| [`townhall-roadmap.md`](https://github.com/Coding-Skuy/ChefGenie-TownHall/blob/main/townhall-roadmap.md) | Batas context (§2) dan Definisi selesai (§6) |

Dokumen turunan repo ini: `versions/v1.0.0/docs/40-TECHSTACK.md` dan `versions/v1.0.0/docs/60-BLAST-RADIUS.md`; keputusan di `docs/decisions/`.

## Tech stack terkunci

- CI (GitHub Actions di `.github/workflows/ci.yml`): `cargo test` (backend), `bun install` + uji (web), build APK KMP, evaluasi bank 12/12 (QA gate).
- Deploy per env: `docker-compose.yml` (lokal: db + backend + pipeline) dan `k8s/` (Deployment backend + StatefulSet Postgres); Postgres 16.x dengan DB logis `pawonee`.
- Secrets per env: `DATABASE_URL`, kunci JWT (audiens `pawonee`), service key server-ke-server (rotasi 90 hari), API key per perangkat; tidak pernah di repo.
- Rilis mobile: Play Store (internal) + APK sideload + TestFlight; MSIX/EXE Windows TIDAK berlaku untuk Pawonee.
- Runbook deploy + rollback terdokumentasi; matriks versi `VERSIONS.md` (kondisi saat ini masih memuat Node 22.12.0 + Next.js 15.1.6).

Larangan: menyimpan secret di repo; service key yang membuka DB divisi lain; deploy tanpa QA gate hijau; mengubah kontrak data modul bersama.

## Invarian domain Pawonee (terkunci v1.0.0)

- **12 resep inti** (`R-01`..`R-12`: tempe-tumis-bawang, sayur-bening-bayam, telur-balado, nasi-goreng-kampung, sop-ayam-sederhana, tumis-kangkung, semur-tahu, pecel-sayur, ayam-kecap-rumahan, sop-jagung-telur, sambal-goreng-tempe-kentang, capcay-kuah). File lama 24 resep berstatus SUPERSEDED (KONTEKS §8.4); dilarang menambah resep ke-13 pada v1.0.0 (PRD 30 Non-Goals; perluasan bank masuk v2.0.0).
- `kodeGrade` ∈ {`G2-V`, `G2-U`, `G2-S`, `NON-G2`}. G2-S dengan kedaluwarsa ≤2 hari mendapat bonus skor +5, tidak boleh diskala untuk esok, dan sisanya dialihkan ke resep sapu stok (KONTEKS §5 menyebut R-24 capcay sapu stok; pada bank 12 resep resep itu adalah `capcay-kuah`, `R-12`).
- Porsi **1–8 jiwa** (faktor 0,30 / 0,55 / 0,80 / 1,00 / 1,25 / 1,50 / 2,00 untuk 1/2/3/4/5/6/8 jiwa); di luar rentang → `JIWA_DI_LUAR_RENTANG`. Biaya per porsi keluarga 4 jiwa **Rp18.000–Rp45.000**.
- Berat gram integer, uang rupiah integer, waktu menit integer. Satuan sah: 13 kode baku Pedaree (KONTEKS §6).
- **PEMILIK** modul `:shared:pantry-resep` (`id.chefgenie.pawonee:pantry-resep:1.0.0`) adalah Pawonee; Pedaree hanya mengonsumsi.
- DB logis `pawonee`; JWT akses 15 menit + refresh 7 hari, audiens `pawonee`.
- Metrik keberhasilan per keluarga per minggu: serapan G2 ≥ **1.500 g**, rasio serapan ≥ **80%**, waste dapur ≤ **300 g**.
- QA gate: evaluasi bank resep **12/12** lolos per rilis (bukti `eval/metrics.md` di `pawonee-ai-models`); rilis diblokir bila gagal.
- Resep yang mengklaim pasokan agregasi Lumbung wajib memakai komoditas pilot (cabai, bawang, kangkung); kentang/wortel/tomat/kol hanya lewat jalur grade-2 pasar (KONTEKS §4).
- Rekomendasi: maksimal 5 item, on-device <800 ms pada HP RAM 3 GB; paritas Kotlin lawan TypeScript selisih <0,01 pada 32 kasus.

## Aturan kerja AI

1. **Baca dulu**: baca `AGENTS.md` ini dan semua ADR di `docs/decisions/` sebelum menulis kode.
2. **TDD merah-hijau**: tulis uji gagal dulu, lalu implementasi. Rilis diblokir bila QA gate gagal (SRS QA-001..QA-004; evaluasi bank 12/12).
3. **Kontrak berversi**: kontrak API/event hanya diubah lewat bump versi (`X-Kontrak-Versi`) dan dicatat di ADR baru; event baru atau perubahan skema memakai masa dual-read 30 hari (KONTEKS §9).
4. **Commit & CHANGELOG**: awalan `docs:`/`feat:`/`fix:`; setiap commit yang mengubah perilaku wajib memperbarui `CHANGELOG.md` repo.
5. **ADR immutable**: keputusan arsitektur baru = tambah ADR bernomor di `docs/decisions/` (1 keputusan per file, berurutan). ADR lama tidak diedit; ganti dengan ADR baru berstatus "Menggantikan ADR-NNNN".
6. **Isolasi data**: DB per service. Dilarang menyentuh DB divisi lain; Pedaree dibaca hanya lewat API/event-nya; Titeny hanya membaca (read-only) dari DB sumber.
7. **Tanpa duplikasi requirement**: requirement berubah = PR di `Pawonee-TownHall` dulu, baru kode mengikuti.
8. **Dokumen beku**: `versions/v1.0.0/` tidak diedit setelah disetujui; perubahan masuk `v1.1.0`/`v2.0.0`.
9. **Secrets**: tidak ada secret di git; rotasi service key 90 hari dan dicatat di runbook.
10. **Gerbang CI**: build merah atau evaluasi bank <12/12 memblokir rilis; rollback harus terdokumentasi sebelum deploy produksi.

## Definisi selesai

Dari [townhall-roadmap §6](https://github.com/Coding-Skuy/ChefGenie-TownHall/blob/main/townhall-roadmap.md) (per milestone app):

- Dokumen versi lengkap (BRD/FRD/FSD/PRD/SRS) + `SNAPSHOT-ROADMAP.md`.
- QA gate berbasis SRS lulus (TDD merah-hijau).
- Kontrak API terbit; audit keamanan bersih (JWT audiens + DB per service).
- Bukti metrik lintas-divisi (KONTEKS §7) tercatat.
- CHANGELOG versi diperbarui; dokumen beku tidak diedit.

Khusus repo ini: CI QA gate berjalan (cargo test, bun, APK KMP, eval 12/12), secrets per env + runbook rollback terdokumentasi, jalur rilis mobile beta berfungsi.
