# File Permissions Audit

**Date:** 26 May 2026

## Sensitive files inspected

| File | Permissions | Owner:Group | Risk | Correct setting |
|------|-------------|-------------|------|-----------------|
| `/opt/bpbpies/config.py` | `-rw-r-----` (640) | `darren_webdev:www-data` | Acceptable – only owner and web group can read. | No change needed. |
| `/opt/bpbpies/database/bee_pies.db` | `-rw-r-----` (640) | `darren_webdev:www-data` | Acceptable. | No change needed. |
| `/home/maureen/.bash_history` | `-rw-------` (600) | `maureen:maureen` | Good – only Maureen can read/write. | No change needed. |
| `/home/maureen/Documents/insurance/claim_draft_v3.docx` | `-rw-------` (600) | `maureen:maureen` | Appropriate for private document. | No change needed. |

## Findings
- All sensitive files have restrictive permissions (600 or 640).
- No world‑readable sensitive files detected.
- The system already enforces least privilege on file access.