# Access Control Policy (ACP)
## NIST CSF 2.0 Control: PR.AC-1 - Identities are managed, authenticated, and authorised.
## Version: 1.0
## Who Wrote This: Governance & Risk Lead (Howard Wai)
## Date: 24-05-2026

### 1. Purpose
This policy defines how user identities, authentication, are managed within the Bee Pies and Pasties system to prevent unauthorised access to customer data, interna files, and proprietary recipes.

### 2. Scipe
This policy applies to:
- The Bee Pies ordering system.
- The server that's hosting the application.
- All staff accounts with system access.
- Any of the third-party contractors with temporary access.

### 3. Policy Statements
1. No shared accounts are permitted. Each user must have an individual account.
2. Least privilege must be applied. Users receive only the permissions required for their specific role.
3. Password Requirements for the Accounts:
- Minimum of 10-12 characters.
- Must include the following upper case, lower case, numbers, and symbols.
- Must not be reused accross the system or on different accounts.
4. Privilege escalation, such as sudo requires authentication.
5. Account reviews must be done every 30 days or less.
6. Dormant accounts that are inactive for 30 days must be disabled.
7. Admin access requires explicit approval from the business owner.

### 4. Access Control Matrix

| Roles    | System Component         | Permissions                  |
|----------|--------------------------|------------------------------|
| Admin    | Server DS                | Full with Sudo               |
| Admin    | Ordering System Backend  | Full                         |
| Staff    | Ordering System Backend  | Read/Write Orders            |
| Staff    | Server DS                | No Access                    |
| Customer | Ordering System Frontend | Limited to Place/View Orders |

### 5. Enforcement
- Violations of this policy may result in an account suspension.
- Admins must maintain logs of account creation, modification, and deletion.

### 6. Review Cycle

This policy will be reviewed regularly or after any cyber security incident.