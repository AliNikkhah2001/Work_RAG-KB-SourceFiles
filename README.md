# Work_RAG-KB Source Files

Private archive of ICS Credit Scoring KB source documents.

## Contents

| Path | Description | Size |
|------|-------------|------|
| archives/KB_31Tir1405_clean.zip | Primary KB archive -- 78 XLSX (clean_files) | ~1.8 MB |
| KB-Source.zip | Same archive at repo root (convenience) | ~1.8 MB |
| clean_files/ | Extracted XLSX files (78 files, UTF-8 names) | -- |

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
