# Work_RAG-KB Source Files

Private archive of ICS Credit Scoring KB source documents.

## Dataset folders (one snapshot per date — ingest exactly one)

| Path | Description |
|------|-------------|
| `1405-06-23/` | **CURRENT** — 41 files (`حقوقی/`, `حقیقی/`, `سایر/`, `ضمیمه پایگاه دانش چت بات/`) → 67 docs / 3,330 chunks in PG |
| `1405-05-31/` | Previous snapshot (34 docs / 2,074 chunks) — kept for history |
| `KB_9.7.2026/` | Older snapshot (22 files) — kept for history |
| `archives/` | Original zips (`KB_31Tir1405_clean.zip`, legacy 78-XLSX set) |
| `*.zip` | Root-level convenience copies of the snapshots |

Legacy flat `clean_files/*.xlsx` were removed (identical content lives in
`archives/`); ingest reads a version folder, e.g.:

```bash
python -m kb_manager.cli ingest --source-dir ../kb-source/1405-06-23 --full
```

## Usage in main repo (Work_RAG-KB)

This repo is included as a **git submodule** at:

Work_RAG-KB/
  kb-source/          -> submodule -> Work_RAG-KB-SourceFiles
    archives/KB_31Tir1405_clean.zip
    KB-Source.zip
    clean_files/*.xlsx

Clone with submodule:

git clone --recurse-submodules https://github.com/AliNikkhah2001/Work_RAG-KB.git

Ingest:

python -m kb_manager.cli ingest --source-dir ../kb-source/clean_files --full

Notes:

* Filenames use UTF-8 (Persian). ZIP was created with ZIP_UTF8 flag (0x800).
* clean_files/ is source of truth after de-duplication.

Version: Source date 31 Tir 1405
