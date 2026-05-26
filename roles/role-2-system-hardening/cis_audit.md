
# CIS Ubuntu 24.04 LTS Benchmark – IG1 Controls

**Audit date:** 26 May 2026

## Control 5.3 – Password expiry policy

**Before remediation**

**Command:** `grep PASS_MAX_DAYS /etc/login.defs`  
**Output:** `PASS_MAX_DAYS   99999`  
**Verdict:** **FAIL** – no password expiry enforced.

**Command:** `sudo chage --list maureen | grep "Password expires"`  
**Output:** `Password expires: never`  
**Verdict:** **FAIL**

**Command:** `sudo chage --list darren_webdev | grep "Password expires"`  
**Output:** `Password expires: never`  
**Verdict:** **FAIL**

**Remediation applied:**  
`sudo chage -M 90 maureen`  
`sudo chage -M 90 darren_webdev`

**After remediation (verification)**

**Command:** `sudo chage --list maureen | grep "Password expires"`  
**Output:** `Password expires: Aug 03, 2026`  
**Verdict:** **PASS**

**Command:** `sudo chage --list darren_webdev | grep "Password expires"`  
**Output:** `Password expires: Aug 03, 2026`  
**Verdict:** **PASS**

---

## Control 5.4 – No empty passwords

**Command:** `sudo awk -F: '($2==""){print $1}' /etc/shadow`  
**Output:** (no output)  
**Verdict:** **PASS** – no accounts with empty passwords.  
**Remediation:** Not required.

---

## Control 5.6 – Root SSH login disabled

**Before remediation**

**Command:** `grep PermitRootLogin /etc/ssh/sshd_config`  
**Output:** `#PermitRootLogin prohibit-password` (commented out)  
**Verdict:** **FAIL** – root login not explicitly disabled.

**Remediation applied:**  
`sudo sed -i 's/^#PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config`  
`sudo systemctl restart ssh`

**After remediation (verification)**

**Command:** `grep PermitRootLogin /etc/ssh/sshd_config`  
**Output:**  
```
PermitRootLogin no
# the setting of "PermitRootLogin prohibit-password".
PermitRootLogin no
PermitRootLogin no
```
**Verdict:** **PASS** – root login is disabled (first uncommented line is `PermitRootLogin no`).

---

## Summary

| Control | Initial verdict | Remediation applied | Final verdict |
|---------|----------------|---------------------|----------------|
| 5.3 – Password expiry | FAIL | `chage -M 90` for both users | PASS |
| 5.4 – No empty passwords | PASS | None needed | PASS |
| 5.6 – Root SSH login disabled | FAIL | Set `PermitRootLogin no | PASS |
