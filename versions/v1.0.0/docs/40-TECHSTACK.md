> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# 40-TECHSTACK — Tumpukan Teknologi (pawonee-infra-devops)

Mengacu: Pawonee-TownHall v1.0.0 (https://github.com/Coding-Skuy/Pawonee-TownHall).

## 1. Kondisi saat ini (tercatat)

- Postgres 16.4, compose db dan backend dan pipeline, CI validasi bank resep dan build backend dan cek web (sumber: `docker-compose.yml`, `VERSIONS.md`, `README.md`).

## 2. Target standar emas (rencana)

- Postgres 16.x terbaru, image backend `ghcr.io/coding-skuy/pawonee-backend-service:0.1.0` naik mengikuti rilis, matriks versi di `VERSIONS.md` diselaraskan ke standar emas tiap repo.
- Selaras divisi: KMP Kotlin 2.2.20 dan Compose 1.8.2 dan nav3 1.0.0; Rust axum 0.8.4; Python 3.12; web SvelteKit dan Bun terbaru; Postgres 16.x; database `pawonee`; JWT audiens `pawonee`.

## 3. Langkah penyesuaian (rencana — bukan eksekusi sekarang)

1. Kunci versi eksak pada `docker-compose.yml` dan `k8s/` saat fase coding dimulai.
2. Selaraskan matriks `VERSIONS.md` di repo ini setelah fase coding dimulai.
3. Jalankan pemeriksaan compose dan migrasi kering sebelum menaikkan versi.
4. Catat perubahan versi pada dokumen ini dan TownHall Pawonee-TownHall v1.0.0.

## Batasan

- Dokumen versi ini tidak mengubah compose, manifes k8s, atau CI; semua target adalah rencana.
- Tidak ada migrasi framework atau kenaikan versi dalam dokumen ini.
- Model tetap mengikuti `:shared:pantry-resep` milik Pawonee; tidak ada duplikasi model baru.
- Perencanaan BRD, PRD, FSD, dan roadmap repo ini mengacu Pawonee-TownHall v1.0.0 dan tidak diduplikasi di sini.
