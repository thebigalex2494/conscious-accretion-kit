---
description: "Taxonomía de clasificación de archivos para organización automática"
globs: "**"
---

# File Classification Taxonomy

## Code Files

| Extension | Category | Destination |
|-----------|----------|-------------|
| .ps1 | Scripts | Projects/scripts/maintenance |
| .py | Tools | Projects/code/tools |
| .ts, .tsx, .js, .jsx | Web | Projects/code/web |
| .json, .yaml, .yml | Config | Projects/config |
| .md | Docs | Projects/docs |
| .sql | Database | Projects/code/database |

## Document Patterns

| Pattern | Category | Destination |
|---------|----------|-------------|
| *factura*, *invoice* | Financial/Invoices | Documents/Financial/Invoices |
| *estado*cuenta*, *statement* | Financial/Statements | Documents/Financial/Statements |
| *cotizacion*, *presupuesto* | Financial/Quotes | Documents/Financial/Quotes |
| *contrato*, *contract* | Legal/Contracts | Documents/Legal/Contracts |
| *reporte*, *report* | Reports | Documents/Reports |
| *cv*, *resume* | Personal/CV | Documents/Personal/CV |

## Media Files

| Extension | Destination |
|-----------|-------------|
| .jpg, .png, .gif, .webp | Pictures |
| .svg | Pictures/Vectors |
| .mp4, .mov | Videos |
| .mp3, .wav | Music |

## Classification Priority
1. Sensitive patterns first (`*password*`, `*.pem`, `*.key`, `.env`) → Flag for review
2. Executable files → Security consideration
3. By extension
4. By filename pattern
5. Fallback → Uncategorized

## Special Rules
- Files >100MB: log separately, suggest compression, don't auto-move
- Duplicates: compare size+date, skip if identical, rename with suffix if different
- Sensitive files: NEVER auto-move, always flag for manual review
