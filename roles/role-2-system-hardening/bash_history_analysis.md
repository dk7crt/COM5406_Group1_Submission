# Bash History Analysis – Maureen Account

**File:** `/home/maureen/.bash_history`  
**Modification timestamp:** Within attack window (02:19 UTC, 6 Nov 2025)


## Incriminating commands (with timestamps)

| Timestamp (epoch) | Command | Significance |
|-------------------|---------|--------------|
| 1762393642 | `sudo cat /etc/shadow` | Privilege escalation reconnaissance. |
| 1762393700 | `sudo -u darren_webdev git config user.name "maureen_bpboss"` | Identity manipulation to commit as Maureen. |
| 1762393720 | `sudo -u darren_webdev git config user.email "maureen@beepiesandpasties.co.uk"` | Same. |
| 1762393880 | `sudo nano /opt/bpbpies/config.py` | Edited config (added comment line). |
| 1762393927 | `git commit -m "update db config"` | Committed the change. |
| 1762394300 | `sudo nano /opt/bpbpies/.env` | Created `.env` with AWS keys. |
| 1762394323 | `git commit -m "add env config"` | Committed secrets. |
| 1762395100 | `sudo nano /tmp/index.html` | Created defacement HTML. |
| 1762395251 | `sudo cp /tmp/index.html /opt/bpbpies/templates/index.html` | Replaced homepage. |
| 1762395393 | `git commit -m "update homepage"` | Committed new homepage (BarmBuzz notice). |
| 1762395500 | `git rm --cached .env` | Removed `.env` from Git tracking. |
| 1762395510 | `sudo rm /opt/bpbpies/.env` | Deleted the file from disk. |
| 1762395527 | `git commit -m "remove env config"` | Cover‑up commit. |
| 1762395600 | `history -c` | Cleared bash history – deliberate tampering. |

## Forensic value
- This history proves that the `maureen_bpboss` Git commits were made from the server, not a local workstation.
- The sequence shows preparation, execution, and cover‑up.
- The `history -c` command indicates intent to hide evidence.