# Acceptable Use Policy – Insider Threat Controls

## Control 1 – Pre‑employment screening (Prevent)
**Risk:** Hiring an insider with malicious intent.  
**Control:** Background checks for all employees with system access, verified by HR before account creation.  
**Verification:** Signed declaration retained in personnel file.

## Control 2 – Mandatory vacation (Detect)
**Risk:** An insider performing unauthorised actions while covering their tracks.  
**Control:** Each employee with privileged access must take at least five consecutive working days of leave annually, during which their access is audited.  
**Verification:** Audit logs reviewed during that period; any suspicious activity recorded.

## Control 3 – Separate administrative accounts (Limit)
**Risk:** Privilege escalation using a standard user account.  
**Control:** Administrative tasks require a separate, named account with no internet access. Regular user accounts have no `sudo` rights.  
**Verification:** `sudo -l` shows no privileges for standard accounts; admin accounts are audited separately.

## Control 4 – Logging and alerting (Respond)
**Risk:** An insider deleting logs to hide actions.  
**Control:** Centralised logging with write‑once storage (e.g., remote syslog). Alerts for `sudo` usage, `.bash_history` clearing, or access to sensitive files (`/etc/shadow`).  
**Verification:** Weekly review of alerts; test by triggering a test alert.