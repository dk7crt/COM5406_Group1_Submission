# User Account Audit – Bee Pies Server

**Date:** 26 May 2026  
**Auditor:** System Hardening Lead

## Accounts with login shells (`/bin/bash`)

| Account | UID | Group memberships | Necessary? | Notes |
|---------|-----|-------------------|------------|-------|
| `darren_webdev` | 1001 | `darren_webdev`, `www-data` | Yes (application owner) | Weak password (`darren2025!`) found in README. |
| `maureen` | 1002 | `maureen`, `sudo`, `docker` | Yes (business owner) | Added to `sudo` group – elevated privileges. |
| `ubuntu` | 1000 | `ubuntu`, `adm`, `sudo` | Yes (lab environment) | Default installer account. |

## Unnecessary accounts
- None. All accounts have a legitimate purpose in the lab context.

## Weak authentication
- `darren_webdev` password (`darren2025!`) is weak and stored in plaintext in README.
- `maureen` password (`Horwich1984!`) is guessable (local reference).

## Remediation
- Enforce strong password policy (CIS 5.3).
- Remove `maureen` from `docker` group unless needed (docker socket privilege escalation risk).