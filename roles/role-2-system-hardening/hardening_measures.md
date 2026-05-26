# Hardening Measures – Before/After Comparison

## Measure 1: Disable debug mode in production

**Before:** `DEBUG = True` in `config.py` line 11  
**Risk:** Leaks stack traces, enables interactive debugger, could lead to remote code execution.  
**After:** `DEBUG = False`  
**Command:** `sed -i 's/DEBUG = True/DEBUG = False/' /opt/bpbpies/config.py`  
**Verification:** `grep DEBUG /opt/bpbpies/config.py` → `DEBUG = False`  
**Attack prevented:** Prevents information disclosure and remote debugger access.

## Measure 2: Remove hardcoded secrets to environment variables

**Before:** `SECRET_KEY = 'bpbpies_secret_key_2024'` and `DB_PASSWORD = 'freddibnah1938'` in `config.py`  
**Risk:** Any attacker with file read access obtains credentials.  
**After:** Replace with `os.getenv('SECRET_KEY')` and `os.getenv('DB_PASSWORD')`; set environment variables in systemd service file.  
**Verification:** `grep -E "SECRET_KEY|DB_PASSWORD" config.py` → shows `os.getenv` calls.  
**Attack prevented:** Credentials are not stored in source code.

## Measure 3: Enforce password expiry policy (CIS 5.3)

**Before:** `PASS_MAX_DAYS 99999` (passwords never expire)  
**Risk:** Compromised credentials remain valid indefinitely.  
**After:** `PASS_MAX_DAYS 90` for `maureen` and `darren_webdev`  
**Command:** `sudo chage -M 90 maureen` and `sudo chage -M 90 darren_webdev`  
**Verification:** `sudo chage --list maureen | grep "Password expires"` → shows expiry date.  
**Attack prevented:** Limits lifetime of compromised credentials.