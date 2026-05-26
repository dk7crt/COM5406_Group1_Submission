# Role Evidence Document – System Hardening Lead

**Consultant:** Adeena Fayyaz  
**Group:** 1  
**Role:** System Hardening Lead

## Session 1A (Week 1) – [date]
- Contributed to initial dossier review.
- Identified private IP `192.168.1.105` as internal.
- Participated in STRIDE worksheet.

## Session 1B (Week 1) – [date]
- Analysed nginx access log for defacement attempt.
- Resolved that the attacker IP is private (insider).
- Committed initial network sketch and STRIDE table.

## Session 2A (Week 2) – [date]
- Reviewed `config.py` and identified hardcoded credentials (`SECRET_KEY`, `DB_PASSWORD`, `DEBUG = True`).
- Completed SHL-1 to SHL-3 in worksheet.
- Documented error handling and configuration findings.

## Session 2B (Week 2) – [date]
- Ran Bandit and interpreted output.
- Performed git forensics: `git blame`, `git log`, `git show`.
- Found `.env` commit (592874a) and deletion (9ec5812).
- Noted tampering: deleted comment in `config.py`.

## Session 3A (Week 3) – [date]
- Participated in SQL injection exploitation (admin login bypass with `' OR 1=1--`).
- Demonstrated IDOR vulnerability on `/order/<order_id>`.
- Documented error response analysis and forensic value of `last_login`.

## Session 3B (Week 3) – [date]
- Audited user accounts and file permissions.
- Examined `/home/maureen/.bash_history` – found incriminating commands.
- Ran three CIS IG1 controls (5.3, 5.4, 5.6).
- Produced before/after hardening comparisons.

## Session 4A (Week 4) – [date]
- Analysed pcap file (`barmbox_capture.pcap`) using `tshark`.
- Identified web searches for "insurance investigation" and "steeplejack training".
- Located DHCP lease table tying `192.168.1.105` to `MAUREEN-LAPTOP`.

## Session 4B (Week 4) – [date]
- Generated SBOM and ran `grype` scan – no critical CVEs found.
- Performed Git history forensics on `.env` – recovered deleted secrets.
- Updated hypothesis with evidence of insurance fraud.

## Session 5A (Week 5) – [date]
- Completed STRIDE synthesis – all artefacts point to insider (H-INT).
- Finalised hardening recommendations.
- Prepared for live demonstration.

## Artefacts committed in this role folder
- `user_account_audit.md`
- `file_permissions_audit.md`
- `bash_history_analysis.md`
- `cis_audit.md`
- `hardening_measures.md`
- `role_evidence_document.md`
- (plus screenshots, bandit outputs, etc. as needed)