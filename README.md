# Work_RAG-KB Source Files

Private archive of ICS Credit Scoring KB source documents, versioned with **DVC**.

## Dataset folders (one snapshot per date — ingest exactly one)

| Path | Description |
|------|-------------|
| `1405-06-23/` | **CURRENT** (see `.dvc_pins/current.txt`) |
| `1405-05-31/` | Previous snapshot — history |
| `1405-07-06/` | Alternate snapshot — history |
| `archives/` | Original zips |

## DVC

Corpus folders are tracked by DVC (not Git blobs). Pointers: `*.dvc`.

```bash
# Pull data after clone
dvc pull -r localremote   # laptop default: ./dvc-storage
# Optional MinIO (compose profile):
#   COMPOSE_PROFILES=minio docker compose ... up -d minio minio-init
#   dvc push -r minio / dvc pull -r minio
```

Default remote: `localremote` → `./dvc-storage`  
Optional remote: `minio` → `s3://kb-corpus/raw` at `http://127.0.0.1:9000` (credentials in `.dvc/config.local`, never commit).

## Ingest into kb-manager

```bash
python -m kb_manager.cli ingest --source-dir ../kb-source/1405-06-23 --full
# then build+activate IndexManifest via UI /registry or:
# curl -X POST http://127.0.0.1:8000/registry/api/manifests/build?activate=true
```

## Processed snapshots (DVC)

Chunks (no vectors), embedding vectors, entity lists, and the pinned prompt/taxonomy packs are exported from the live KB database and tracked by the `snapshot-processed` stage. Git stores `dvc.lock`; the bytes live in the DVC remote. The active corpus date is `.dvc_pins/processed.txt`.

```bash
dvc repro snapshot-processed   # needs KB Postgres on 127.0.0.1:15432
dvc push -r localremote
# KB manager reads packs/dvc_snapshot.json and serves GET /registry/api/dvc
```

## Submodule usage

Included from Work_RAG-KB at `kb-source/`.
