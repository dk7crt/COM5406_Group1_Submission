
# Attack Surface Map  
## Bee Pies & Pasties — Role 3 Network and Threat Modelling

**Name:** Maduabuchi Victor Orizu  
**Module:** COM5406 Cyber Security  

---

## 1. Purpose

This document summarises the Bee Pies attack surface before hardening. It is based on PCAP evidence, DHCP records, host findings and application vulnerabilities.

The aim is to identify where an attacker could enter, move through, or misuse the system, while separating direct evidence from inference.

---

## 2. Key Entry Points

| Entry Point | Security Relevance |
|---|---|
| HTTP port 80 | Cleartext traffic was visible in the PCAP, supporting Information Disclosure. |
| Flask application routes | SQL injection and broken access control created possible application-level attack paths. |
| Staff Wi-Fi `BPBPies-Staff` | PCAP metadata links activity to the internal staff network. |
| Internal LAN devices | `192.168.1.105` was active and mapped by DHCP to `MAUREEN-LAPTOP`. |
| Local user accounts | Accounts such as `root`, `ubuntu`, `maureen` and `darren_webdev` increase the host-level attack surface. |
| Git/deployment path | The defacement mechanism is not fully proven, but deployment or server-side modification remains possible. |

---

## 3. Trust Boundaries

| Trust Boundary | Risk |
|---|---|
| Staff Wi-Fi → Internal LAN | A compromised or misused staff device could access internal services. |
| Internal LAN → `BAKERY-SERVER` | Internal devices could interact with the Bee Pies application/server. |
| Web application → Database | SQL injection or weak access control could expose or alter data. |
| User account → File system/deployment | Account misuse could support website modification or defacement. |

---

## 4. Main Attack Surfaces

### Network

- Cleartext HTTP traffic visible in the PCAP.
- Staff Wi-Fi metadata exposed:

```text
SSID=BPBPies-Staff
LOCATION=inside_bakery
````

* Internal source activity from `192.168.1.105`.

### Application

* SQL injection risk.
* Broken access control.
* Exposed Flask routes.
* Suspicious `PUT /index.html` request, although it returned `405 Method Not Allowed`.

### Host

* Hardcoded database password:

```text
DB_PASSWORD = 'freddibnah1938'
```

* Multiple local user accounts.
* Possible account, permission, or deployment misuse.

### Data

* Customer/order data.
* Recipe/archive data.
* Credentials and configuration files.
* Website content, including `/index.html`.

---

## 5. Highest Risk Areas

| Risk Area             | STRIDE Link                                                | Reason                                                               |
| --------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------- |
| Cleartext HTTP        | Information Disclosure                                     | Requests were readable in the PCAP.                                  |
| SQL injection         | Tampering / Information Disclosure                         | Could expose or manipulate database content.                         |
| Broken access control | Information Disclosure / Elevation of Privilege            | Users may access data or functions they should not.                  |
| Hardcoded DB password | Spoofing / Information Disclosure / Elevation of Privilege | Exposed credentials may allow unauthorised access.                   |
| Website defacement    | Tampering                                                  | The site was altered, although the exact method needs correlation.   |
| Weak attribution      | Repudiation                                                | IP/device evidence does not prove the human actor.                   |
| Staff Wi-Fi access    | Spoofing / Tampering                                       | Internal access increases risk if a device or credential is misused. |

---

## 6. Recommended Risk Reductions

* Enforce HTTPS/TLS and disable cleartext HTTP.
* Use parameterised queries to prevent SQL injection.
* Fix broken access control on order/admin functions.
* Remove and rotate hardcoded credentials.
* Segment staff Wi-Fi from server/admin systems.
* Restrict deployment paths and file permissions.
* Improve logging across web, authentication, git and deployment activity.
* Apply least privilege to local accounts and service accounts.

---

## 7. Evidence Limitations

* The PCAP contains only 30 packets, so it does not show the full attack.
* `192.168.1.105` and `MAUREEN-LAPTOP` support device attribution, not human attribution.
* The `PUT /index.html` request was suspicious, but it returned `405`, so it was likely not the direct cause of defacement.
* The exact deployment or server-side modification route remains unconfirmed.
* More evidence is needed from auth logs, sudo logs, bash history, git history and file timestamps.

---

## 8. Conclusion

The Bee Pies system exposed attack surfaces across the network, application, host and data layers.

The strongest evidenced risks are **cleartext HTTP**, **hardcoded credentials**, **SQL injection**, and **broken access control**. These align directly with the STRIDE findings, especially **Information Disclosure**, **Tampering**, **Spoofing**, **Repudiation** and possible **Elevation of Privilege**.

The attack surface map supports a multi-stage incident model: internal access through the staff network, exploitation or misuse of the application/account/deployment path, and eventual website defacement.

However, the evidence still supports **device-level attribution**, not proven human attribution.

```



