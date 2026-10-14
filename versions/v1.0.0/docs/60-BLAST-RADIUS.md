> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# 60-BLAST-RADIUS — Batas Context (pawonee-infra-devops)

Mengacu: Pawonee-TownHall `platform/versions/v1.0.0` (https://github.com/Coding-Skuy/Pawonee-TownHall/tree/main/platform/versions/v1.0.0), beku 2026-10-07. Baris Pawonee townhall-roadmap §2: point of concern "12 resep inti; serapan ≥1.500 g"; batas context "G2-V/U/S; porsi 1–8 jiwa; DB `pawonee`; JWT audiens `pawonee`". Rujukan lintas divisi: KONTEKS §8/§9.

## 1. Boleh disentuh repo ini

- `docker-compose.yml`, `k8s/`, `.github/workflows/`, `VERSIONS.md`, runbook.
- Provisioning DB logis `pawonee` dan secrets per env.

## 2. Dilarang

- Menyimpan secret di repo atau membagikan service key lintas divisi.
- Mengubah kode/kontrak repo lain; hanya membangun dan men-deploy-nya.
- Deploy produksi tanpa QA gate hijau dan runbook rollback.
- Menyentuh DB divisi lain (DB per service; Titeny hanya read-only ke DB sumber).
- Memakai komoditas non-pilot (kentang/wortel/tomat/kol) untuk klaim pasokan agregasi Lumbung; hanya cabai, bawang, kangkung (KONTEKS §4).
- Menambah resep ke-13, mengubah definisi G2, atau mengubah audiens JWT tanpa PR TownHall + ADR.

## 3. Kontrak yang diproduksi

- Pipeline CI/QA gate, artefak APK/TestFlight, environment per env.
- Runbook deploy + rollback; matriks versi `VERSIONS.md`.

## 4. Kontrak yang dikonsumsi

- Repo `pawonee-backend-service`, `pawonee-web`, `pawonee-app-kmp`, `pawonee-ai-models`, `pawonee-data-pipeline`, `pawonee-design`.
- Hasil evaluasi `eval/metrics.md` (12/12) sebagai syarat rilis.

## 5. Dampak bila berubah

- Perubahan CI/secrets dapat memblokir seluruh rilis divisi.
- Perubahan DB/secret yang salah dapat memutus autentikasi (JWT audiens `pawonee`).

## Batasan

- Dokumen ini hanya memetakan batas dan risiko; tidak mengubah kode atau konfigurasi.
- Keputusan lintas divisi dirujuk ke `KONTEKS-LINTAS-DIVISI.md` dan TownHall Pawonee v1.0.0.
