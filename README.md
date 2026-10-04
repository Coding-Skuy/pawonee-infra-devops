# pawonee-infra-devops

Infrastruktur Pawonee — **Divisi Pawonee (AI Cooking Assistant)**, org `Coding-Skuy`. Menjalankan backend Rust, database `pawonee`, pipa sinyal Pedaree, dan CI semua repo. Bagian dari arsitektur **Opsi A ChefGenie**.

- TownHall: [Coding-Skuy/Pawonee-TownHall](https://github.com/Coding-Skuy/Pawonee-TownHall).
- Kontrak data mengikuti modul `:shared:pantry-resep` **milik Pawonee** (sumber: `pawonee-app-kmp/shared/pantry-resep`); infra tidak mengubah kontrak tersebut.
- Konsumen hilir: **Pedaree** menerima sinyal lewat tabel/topik yang diprovisi compose dan manifes k8s di sini.

## Versi dipin (lihat VERSIONS.md)

Kotlin 2.0.21, Node 22.12.0, Next.js 15.1.6, Rust 1.82.0, Python 3.12.7, Postgres 16.4.

## Isi

```
docker-compose.yml            # db pawonee + backend + pipeline (pengembangan lokal)
k8s/backend-deployment.yaml   # Deployment + Service backend
k8s/postgres.yaml             # StatefulSet database pawonee
.github/workflows/ci.yml      # CI: validasi bank resep, build backend, cek web
VERSIONS.md                   # matriks versi standar
```

## Cara jalan lokal

```bash
docker compose up --build
curl http://localhost:8080/kesehatan
```

## Repo yang diorkestrasi

- [pawonee-app-kmp](https://github.com/Coding-Skuy/pawonee-app-kmp)
- [pawonee-web](https://github.com/Coding-Skuy/pawonee-web)
- [pawonee-backend-service](https://github.com/Coding-Skuy/pawonee-backend-service)
- [pawonee-ai-models](https://github.com/Coding-Skuy/pawonee-ai-models)
- [pawonee-data-pipeline](https://github.com/Coding-Skuy/pawonee-data-pipeline)
- [pawonee-design](https://github.com/Coding-Skuy/pawonee-design)
