# Insider Threat Analysis – System Hardening Lead

## Least‑privilege violations (Session 3B)
- User `maureen` in `sudo` and `docker` groups (privilege escalation risk).
- No password expiry policy – credentials never expire.
- Weak password `Horwich1984!` for `maureen`.

## Hardening measures that would have made the activity harder
- **Password expiry (CIS 5.3)** – would have limited the lifetime of her credential.
- **Removing `maureen` from `docker` group** – would block root‑equivalent access via docker socket (not used in this attack, but a latent risk).
- **Root SSH disable (CIS 5.6)** – would not stop her using her own account, but prevents lateral movement after escalation.

## Hardening measures that would NOT have touched the activity
- **Disabling debug mode** – addresses information leakage, not insider use of valid credentials.
- **SQL injection fixes** – the attack used a legitimate account, not SQLi.
- **Moving secrets to environment variables** – she had write access and added `.env` anyway.

## Conclusion
The strongest missing insider‑specific control was **password expiry**. The other hardening measures address different parts of the attack surface.