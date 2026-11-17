# Work_RAG-KB Source Files

Private archive of ICS Credit Scoring KB source documents, versioned with **DVC**.

## Dataset folders (one snapshot per date — ingest exactly one)

| Path | Description |
|------|-------------|
| `1405-08-20/` | **CURRENT** raw corpus (see `.dvc_pins/current.txt`) |
| `1405-07-06/` | Previous tree — history |
| `1405-06-23/` | Previous snapshot — history |
| `1405-05-31/` | Older snapshot — history |
| `archives/` | Original zips, including `1405-08-20.zip` |
| `processed/1405-08-20/` | **CURRENT** embedded snapshot (chunks + vectors + entity lists) |

## DVC

Corpus folders and processed snapshots are tracked by DVC (not Git blobs). Git stores `*.dvc` / `dvc.lock` pointers.

```bash
# Pull raw + processed after clone (needs a populated DVC remote)
dvc pull -r localremote
```

Default remote: `localremote` → `./dvc-storage` (laptop). Optional MinIO: `dvc pull -r minio`.

## Processed snapshot (embedded KB) — GitHub Release

Live Postgres is **not** in Git. After ingest we export JSONL + embeddings and publish a Release asset so another machine can restore without re-embedding.

**Current pin:** `.dvc_pins/processed.txt` → `1405-08-20` (21 docs / 1319 chunks / 8 entity lists).

### On the laptop (after ingest)

```bash
cd components/knowledgebase/kb-source
dvc repro snapshot-processed          # or: PYTHONPATH=../kb-manager python3 -m kb_manager.dvc_snapshot export --kb-source .
dvc push -r localremote
# then publish Release (CI/agent): processed-1405-08-20.tar.gz
```

### On the server (restore DB from GitHub)

```bash
cd components/knowledgebase/kb-source
git pull
gh release download processed-1405-08-20 \
  -R AliNikkhah2001/Work_RAG-KB-SourceFiles \
  -p 'processed-1405-08-20.tar.gz' -D /tmp
mkdir -p processed
tar -C processed -xzf /tmp/processed-1405-08-20.tar.gz
# Postgres must be empty or you accept upserts:
PYTHONPATH=../kb-manager python3 -m kb_manager.dvc_snapshot restore \
  --kb-source . --corpus 1405-08-20 --apply \
  --db-url 'postgresql://postgres:postgres@127.0.0.1:5432/kb_manager'
# restart KB so BM25/dense reload
```

## Ingest from raw (alternative to restore)

```bash
python -m kb_manager.cli ingest --source-dir ../kb-source/1405-08-20 --full
```

## Submodule usage

Included from Work_RAG-KB at `kb-source/`.
