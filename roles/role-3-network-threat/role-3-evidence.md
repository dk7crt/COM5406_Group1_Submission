# Role 3 Evidence Document - Network and Threat Modelling Lead
## Name: Madu
## Role: Network and Threat Modelling Lead
## Case: Bee Pies & Post-Incident Investigation
## Module: COM5406 Cyber Security


--
## 1. Role Scope
My role is Network and Threat Modelling lead. I am responsible for analysing the network evidence, maintaining the STRIDE threat model, mapping the attack surface, and explaining how the network evidence connects to the wwider incident narrative.

The main artefacts for my role are:
-Network evidence analysis
-STRIDE threat model
-Network topology diagram
-Attack surface map
-Attack path diagram
-PCAP analysis
-Live demo notes

My approach  is to separate direct evidence from inference. I do not treat an IP address, git author name, or suspected actor as proof of identity unless it is supported by other evidence.

---

## 2. Session 1A — Initial Dossier Review and STRIDE Worksheet

### What I did

In Session 1A, I reviewed the initial Bee Pies & Pasties incident evidence from a network and threat-modelling perspective. I focused on the first suspicious access log entry, the CIA triad, STRIDE, and the initial working hypothesis.

### Evidence reviewed

```text
192.168.1.105 - - [15/Mar/2025:02:14:11 +0000] "PUT /index.html HTTP/1.1" 200 612 "-" "curl/7.88.1"
```

### Analysis

The source IP was `192.168.1.105`. At this stage, I treated this as a network identifier only, not as proof of a named person.

The request used the HTTP `PUT` method against `/index.html`. This was significant because `PUT` is a write-oriented method and may indicate an attempt to alter or replace web content.

The user agent was `curl/7.88.1`, which suggested command-line activity rather than ordinary customer browsing.

### Initial CIA assessment

| CIA property    | Asset                                        | Breach meaning                                             |
| --------------- | -------------------------------------------- | ---------------------------------------------------------- |
| Confidentiality | Customer records, order data, recipe archive | Unauthorised access to sensitive business or customer data |
| Integrity       | Homepage, order records, application code    | Unauthorised modification of website content or data       |
| Availability    | Bee Pies ordering system                     | Customers or staff unable to access the service            |

The most immediate concern was **Integrity**, because the evidence showed a request that appeared to target the homepage for modification.

### Initial STRIDE assessment

| STRIDE category        | Assessment        | Reason                                                                                                               |
| ---------------------- | ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| Spoofing               | Possible          | The log shows an IP address but not a verified user identity.                                                        |
| Tampering              | Evidenced         | `PUT /index.html` suggests attempted modification of web content.                                                    |
| Repudiation            | Possible          | The log does not show an authenticated named user.                                                                   |
| Information Disclosure | Not evidenced yet | This single log entry does not show data being read or leaked.                                                       |
| Denial of Service      | Not evidenced yet | No high-volume traffic or outage is shown.                                                                           |
| Elevation of Privilege | Possible          | If the actor could modify web content, they may have had access beyond normal browsing, but this was not proven yet. |

### Working hypothesis

At this stage, my working hypothesis was that a technically capable actor attempted to modify Bee Pies web content during the early-hours incident window.

BarmBuzz Ltd was named in the incident dossier as a suspected actor, but I treated this only as a working hypothesis. The technical evidence did not yet prove BarmBuzz involvement.

### Questions carried forward

1. What device or user was associated with `192.168.1.105`?
2. Did the `PUT /index.html` request actually cause the defacement?
3. What other evidence exists in the pcap, server logs, bash history, and git history?

---

## 3. Session 1B — Network Perimeter and STRIDE v1

### What I did

In Session 1B, I analysed the network perimeter and the significance of `192.168.1.105`. I also contributed to the first STRIDE table and network sketch. The Session 1B worksheet says the Network and Threat Modelling Lead owns the canonical STRIDE table and network sketch.

### IP address analysis

`192.168.1.105` is in the private IPv4 range:

```text
192.168.0.0/16
```

This means it is not publicly routable on the internet. Therefore, the request likely came from inside the Bakery LAN, a compromised internal device, or another internal access path.

At this stage, I labelled it only as:

```text
Unknown device — 192.168.1.105
```

I did not attribute it to a named person because the evidence was not strong enough.

### nginx log analysis

The suspicious log entry contained:

| Field       | Value           | Meaning                    |
| ----------- | --------------- | -------------------------- |
| Source IP   | `192.168.1.105` | Private LAN address        |
| Timestamp   | `02:14:11`      | Within the incident window |
| HTTP method | `PUT`           | Write-oriented request     |
| Resource    | `/index.html`   | Homepage or web content    |
| Status code | `405`           | Method not allowed         |
| User agent  | `curl/7.88.1`   | Command-line HTTP client   |

The `405 Method Not Allowed` response was important. It means the direct HTTP `PUT` request was rejected. Therefore, I treated it as suspicious attempted tampering, but not proof that this exact request caused the defacement.

### STRIDE v1

| STRIDE category        | Evidence state        | Evidence or gap                                                                 |
| ---------------------- | --------------------- | ------------------------------------------------------------------------------- |
| Spoofing               | Possible              | Source IP is known, but user identity is unknown.                               |
| Tampering              | Possible/Evidenced    | `PUT /index.html` suggests attempted tampering, but `405` means it was blocked. |
| Repudiation            | Possible              | No authenticated user identity is shown.                                        |
| Information Disclosure | Insufficient evidence | No data access shown in this log entry.                                         |
| Denial of Service      | Insufficient evidence | No flooding or outage pattern shown.                                            |
| Elevation of Privilege | Insufficient evidence | No proof of privilege escalation yet.                                           |

### Network sketch v1

```text
[Internet]
    |
[Trust Boundary]
    |
[Bakery LAN]
    |
    |---- [Bee Pies Web Server]
    |
    |---- [Unknown Device: 192.168.1.105]
```

### Reflection

Session 1B changed my understanding because the `405` response weakened the idea that the direct `PUT` request caused the defacement. The investigation needed more evidence from pcap, host logs, bash history, git history, and application vulnerabilities.

---

## 4. Session 2A — OWASP Code Reading and Threat Model Update

### What I did

In Session 2A, the group reviewed the Flask application using OWASP categories. My role was to update the threat model based on the application attack surface.

### Network/threat modelling contribution

I identified that the incident could not be understood only from the network log. The web application itself had routes, input fields, credentials, and database interactions that could become attack paths.

The important areas for my role were:

* User-facing web routes
* Order form and order lookup functionality
* Login/admin routes
* Hardcoded credentials
* Database access
* Missing or weak access control
* Weak configuration

### STRIDE update

| STRIDE category        | Update                                                                                                  |
| ---------------------- | ------------------------------------------------------------------------------------------------------- |
| Tampering              | User input reaching database queries could allow unauthorised modification or manipulation of data.     |
| Information Disclosure | Hardcoded credentials or vulnerable routes could expose customer/order data or business-sensitive data. |
| Elevation of Privilege | Credential misuse or authentication weaknesses could allow access beyond normal user permissions.       |
| Repudiation            | Weak logging and weak identity checks could make it difficult to prove who performed actions.           |

### External vs internal attacker model

Under an external attacker model, the attacker would need to reach the exposed web application and exploit a vulnerable route.

Under an internal attacker model, an attacker or compromised device already inside the Bakery LAN would have fewer barriers. This made `192.168.1.105` important because it was a private internal address.

### Reflection

Session 2A expanded my model from a simple “suspicious request” to a wider attack surface. The network path may explain how traffic reached the system, but the application weaknesses may explain how compromise became possible.

---

## 5. Session 2B — Bandit Triage and Git Provenance

### What I did

In Session 2B, the group used Bandit static analysis and git history to investigate code weaknesses and their provenance. My role was to update the STRIDE model using Bandit and git evidence.

### Evidence sources

The evidence sources for this session were:

```text
bandit -r .
git blame config.py
git log config.py
git show <hash> -- config.py
git log --all --oneline
```

### Bandit evidence

The key Bandit finding relevant to my role was:

```text
B105 config.py:12 DB_PASSWORD = 'freddibnah1938'
```

This showed a hardcoded database password in the codebase.

### STRIDE update from Bandit

| Finding                                     | STRIDE category        | Threat model impact                                                                                         |
| ------------------------------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------- |
| Hardcoded database password                 | Information Disclosure | The credential is exposed in source code and may reveal access to sensitive data.                           |
| Hardcoded database password                 | Spoofing               | If valid, the credential could allow an attacker to authenticate as a legitimate application/database user. |
| Hardcoded database password                 | Elevation of Privilege | If the credential gives more access than a normal user, it can increase attacker capability.                |
| Unsafe SQL construction / injection pattern | Tampering              | Attacker-controlled input may alter query behaviour.                                                        |
| Unsafe SQL construction / injection pattern | Information Disclosure | SQL injection may expose customer/order data.                                                               |

### Git provenance analysis

Git history is useful because it shows when a risky line entered the repository and which author identity was attached. However, git author identity alone is not proof of a human actor because git identity can be configured locally.

### Multi-source evidence analysis

| Evidence source | What it shows                                          | What it does not prove                     |
| --------------- | ------------------------------------------------------ | ------------------------------------------ |
| Bandit          | Risky code patterns                                    | Whether they were exploited                |
| Git history     | When code changed and author metadata                  | Who physically made the change             |
| nginx logs      | Requests to the server                                 | Human identity or full intent              |
| pcap            | Network traffic, IPs, protocols, readable HTTP content | Full host activity or definite attribution |

### Reflection

Session 2B strengthened my threat model because it added code and repository evidence. It also showed that no single source is enough. Strong conclusions require correlation between Bandit, git history, nginx logs, pcap evidence, DHCP leases, bash history, and auth logs.

---

## 6. Session 3A — Exploit-and-Patch Contribution

### What I did

In Session 3A, the group investigated SQL injection and broken access control in the live Bee Pies application. My role was to update STRIDE using concrete exploit evidence and map the application attack surface.

### STRIDE update from exploit evidence

| STRIDE category        | Evidence-based update                                                                                             |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Tampering              | SQL injection could allow attacker-controlled input to change query behaviour and possibly modify database state. |
| Information Disclosure | SQL injection and broken access control could expose customer/order data.                                         |
| Elevation of Privilege | Authentication bypass through injection could allow access beyond what an unauthenticated user should have.       |
| Repudiation            | Weak logs, shared credentials, or bypassed authentication weaken attribution.                                     |

### Attack surface mapping

The attack surface now included:

* Admin login route
* Order lookup route
* User input fields
* Database queries
* Session/authentication logic
* Access control checks
* Application logs

The order lookup route was important because type-checking an `order_id` does not prove that the user owns the order. That is a Broken Access Control issue, not just an injection issue.

### Residual exposure

Even if SQL injection is patched, risk may remain if:

* Broken access control is not fixed
* Hardcoded credentials are not removed and rotated
* Similar query patterns exist elsewhere
* Logs do not support strong attribution
* Internal device compromise remains possible

### Reflection

Session 3A was a turning point. The investigation moved from theoretical threat modelling to confirmed exploit paths. The incident model became stronger because application weaknesses provided a more realistic path to compromise than the blocked `PUT` request alone.

---

## 7. Session 3B — Host Evidence, DHCP, STRIDE Update and Attack Path

### What I did

In Session 3B, the group investigated host/server evidence, including user accounts, DHCP leases, file permissions, bash history and sudo/authentication logs. My role was to update STRIDE and start the attack path from network access to defacement.

The Session 3B worksheet required the attack path to include:

```text
device on network → server account → privilege escalation → git repository → defacement
```

### PCAP evidence

The pcap analysed was:

```text
/home/darren_webdev/pcaps/barmbox_capture.pcap
```

Key pcap facts:

```text
Packets: 30
Capture duration: 428.019 seconds
First packet: 2025-11-06 01:30:00 UTC
Last packet: 2025-11-06 01:37:08.019000 UTC
SHA256: bfae5b1c815f4911e65d0bb77b325f9d21f0065ce08e00ac033e9daf12871591
```

Protocol hierarchy:

```text
30 Ethernet frames
4 ARP frames
26 IP packets
7 UDP/DNS packets
19 TCP packets
3 HTTP packets
1 malformed DNS packet
```

HTTP findings:

```text
192.168.1.105 → gmail.com
GET /mail HTTP/1.1
Host: gmail.com
```

```text
192.168.1.105 → www.google.com
GET /?q=how+long+does+insurance+investigation+take+uk+small+business
```

```text
192.168.1.105 → www.google.com
GET /?q=steeplejack+training+courses+lancashire
```

Capture metadata:

```text
CAPTURE_META: SSID=BPBPies-Staff SIGNAL=-42dBm LOCATION=inside_bakery
```

### STRIDE update by Week 3B

| STRIDE category        | Evidence                                                                   | Interpretation                                                                  |
| ---------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Spoofing               | `192.168.1.105` appears in network/log evidence.                           | IP address identifies traffic, not a verified person.                           |
| Tampering              | Site was defaced, but `PUT /index.html` returned `405`.                    | The direct PUT was blocked, so defacement likely happened through another path. |
| Repudiation            | Logs show IPs, accounts and git author fields, but these need correlation. | Attribution remains limited without DHCP, auth logs and bash history.           |
| Information Disclosure | HTTP traffic was readable in pcap; SQLi/access control risks exposed data. | Strongest evidenced STRIDE category.                                            |
| Denial of Service      | Only 30 packets; no flood or outage pattern.                               | Not evidenced in this pcap.                                                     |
| Elevation of Privilege | Host evidence such as sudo/auth.log/bash history is needed.                | Needs correlation with System Hardening evidence.                               |

### Working attack path

```text
1. Device on network
   Evidence: 192.168.1.105 appears in logs and pcap.
   Evidence: CAPTURE_META shows BPBPies-Staff and inside_bakery.

        ↓

2. Server/application interaction
   Evidence: nginx log shows suspicious request using curl.
   Evidence: PUT /index.html returned 405.

        ↓

3. Application vulnerability route
   Evidence: Week 3A confirmed SQL injection and access control weaknesses.

        ↓

4. Server account activity
   Evidence needed: bash history, account audit, login/session evidence.

        ↓

5. Privilege escalation
   Evidence needed: auth.log sudo entries and group membership.

        ↓

6. Git repository or file modification
   Evidence needed: git history, bash history showing git commands, commit timestamps.

        ↓

7. Defacement
   Interpretation: Since PUT returned 405, the successful defacement likely happened through repository, server-side, deployment, or application-level change.
```

### R3-Q1 — Weakest evidence

The weakest step is the link between `192.168.1.105` and the specific human actor/account responsible for later server or git actions.

An IP address alone does not prove who used the device. To strengthen this, I would need DHCP lease records, MAC address/vendor, hostname, authentication logs, bash history and git timestamps.

### R3-Q2 — Reconciling 405 PUT with defacement

The `PUT /index.html` request returned `405 Method Not Allowed`, so that exact request was blocked. Therefore, it is suspicious evidence of attempted tampering, but it is unlikely to be the direct cause of the defacement.

The defacement mechanism likely appears later in the attack path, through application exploitation, shell access, git repository changes, deployment changes, or server-side file modification.

### Reflection

By Week 3B, my understanding changed from a simple theory based on a suspicious `PUT` request to a multi-stage attack model. The stronger model now combines network access, vulnerable application routes, host evidence, possible privilege escalation, git/repository activity and defacement.

The strongest evidenced STRIDE category is Information Disclosure because the pcap showed readable HTTP content and the application vulnerabilities created data exposure risk.

---

## 8. Summary of Progress up to Week 3B

By the Week 3B checkpoint, I had contributed to:

*Initial STRIDE assessment
 Network perimeter analysis
 Network sketch
 OWASP-based STRIDE update
 Bandit and git provenance threat model update
 Exploit-based STRIDE update
 Attack surface mapping
 PCAP analysis
 Working attack path diagram

