
# STRIDE Threat Model  
## Role 3 — Network and Threat Modelling Lead

**Name:** Maduabuchi Victor Orizu  
**Case:** Bee Pies & Pasties Post-Incident Investigation  
**Module:** COM5406 Cyber Security  

---

## 1. Purpose

This document records my current STRIDE threat model for the Bee Pies & Pasties investigation.

My role is to connect network evidence, application weaknesses, host evidence and incident artefacts into a clear threat model. I separate **direct evidence** from **inference**, especially when considering attribution.

Session 4B adds an insider-threat angle: the actor may have used staff Wi-Fi, a trusted device, legitimate credentials, or physical access inside the bakery.

---

## 2. Key Evidence

| Evidence | Relevance |
|---|---|
| `192.168.1.105` | Internal source IP seen in network/log evidence. Identifies a source, not a confirmed person. |
| DHCP mapping to `MAUREEN-LAPTOP` | Supports device attribution, but not human attribution. |
| `PUT /index.html` | Suspicious write-oriented request targeting homepage content. |
| `405 Method Not Allowed` | Shows the direct PUT was blocked, so this request likely did not cause the defacement. |
| `curl/7.88.1` | Suggests command-line activity rather than normal browsing. |
| PCAP: 30 packets, HTTP/DNS | Small capture, but useful for showing network context and cleartext traffic. |
| `CAPTURE_META: SSID=BPBPies-Staff SIGNAL=-42dBm LOCATION=inside_bakery` | Shows staff Wi-Fi and inside-bakery context. |
| `DB_PASSWORD = 'freddibnah1938'` | Hardcoded database password found in code evidence. |
| SQL injection / broken access control | Supports possible application-level attack paths. |

---

## 3. STRIDE Summary

| STRIDE Category | Current Position |
|---|---|
| Spoofing | Possible. A trusted device, account, or exposed credential may have been used to appear legitimate, but the human actor is not proven. |
| Tampering | Evidenced/likely. The site was defaced and `/index.html` was targeted, but the direct `PUT` returned `405`, so the successful method was likely another route. |
| Repudiation | Possible. IPs, hostnames, accounts and git metadata do not prove who physically performed the action. |
| Information Disclosure | Strongest finding. Cleartext HTTP, staff Wi-Fi metadata, hardcoded credentials, SQL injection and broken access control all support this. |
| Denial of Service | Not evidenced. The pcap contains only 30 packets and shows no flooding or outage pattern. |
| Elevation of Privilege | Possible but not proven. It needs stronger support from auth logs, sudo logs, permissions, bash history and deployment evidence. |

---

## 4. STRIDE Analysis

### 4.1 Spoofing

Spoofing is possible because the evidence identifies devices and credentials, not a verified human actor.

`192.168.1.105` and the DHCP mapping to `MAUREEN-LAPTOP` help identify a device, but they do not prove who used it. The hardcoded database password also increases spoofing risk because exposed credentials could allow an attacker to appear legitimate.

**Conclusion:** Spoofing is possible, especially under an insider or compromised-device model, but attribution remains limited.

---

### 4.2 Tampering

Tampering is relevant because the website was defaced and `/index.html` was targeted.

The suspicious request was:

```text
PUT /index.html
````

However, it returned:

```text
405 Method Not Allowed
```

This means the direct PUT was likely blocked. Therefore, it is evidence of attempted tampering, but not proof that this exact request caused the defacement.

Possible successful routes include SQL injection, broken access control, account misuse, server-side file modification, git repository change, or deployment misuse.

**Conclusion:** Tampering is likely, but the exact successful method still needs correlation with application, host and git evidence.

---

### 4.3 Repudiation

Repudiation remains a concern because the evidence does not fully prove the human actor.

IPs, hostnames, git metadata and account names can help build a timeline, but they are not enough on their own. Stronger attribution would require DHCP leases, authentication logs, bash history, git timestamps, file modification times and account/session evidence.

**Conclusion:** Repudiation is possible because the current evidence supports device-level attribution more than human-level attribution.

---

### 4.4 Information Disclosure

Information Disclosure is the strongest evidenced STRIDE category.

The pcap shows readable HTTP traffic, meaning request details could be seen by someone with access to the capture or network path. The capture also includes:

```text
CAPTURE_META: SSID=BPBPies-Staff SIGNAL=-42dBm LOCATION=inside_bakery
```

This reveals staff Wi-Fi and inside-bakery context.

Code evidence also supports Information Disclosure:

```text
DB_PASSWORD = 'freddibnah1938'
```

SQL injection and broken access control could also expose customer/order data.

**Conclusion:** Information Disclosure is strongly evidenced by readable HTTP traffic and supported by exposed credentials and application weaknesses.

---

### 4.5 Denial of Service

There is no strong Denial of Service evidence.

The pcap only contains 30 packets and does not show flooding, repeated failed requests, resource exhaustion, or outage behaviour.

**Conclusion:** Denial of Service is not evidenced.

---

### 4.6 Elevation of Privilege

Elevation of Privilege is possible but not proven.

The hardcoded database password, SQL injection and broken access control could increase attacker capability. However, network evidence alone does not prove privilege escalation.

Stronger evidence would be needed from sudo logs, auth logs, group membership, bash history, file permissions and deployment permissions.

**Conclusion:** Elevation of Privilege remains possible, but it needs stronger host/system evidence.

---

## 5. Updated Attack Model

```text
Internal device / legitimate access
        ↓
Staff Wi-Fi or Bakery LAN
        ↓
Application, account, git or deployment route
        ↓
Website defacement
```

The blocked `PUT /index.html` means the successful defacement likely happened through another route, not the direct PUT request.

---

## 6. Evidence Limitations

* The pcap is small, with only 30 packets.
* `192.168.1.105` identifies a source/device, not a confirmed human.
* `MAUREEN-LAPTOP` supports device attribution, not human attribution.
* The direct `PUT /index.html` returned `405`.
* Git metadata does not prove who physically made a change.
* Host evidence is still needed to confirm account use, privilege escalation and file modification.

---

## 7. Final Conclusion

The strongest evidenced STRIDE category is **Information Disclosure**, supported by cleartext HTTP traffic, exposed staff Wi-Fi context, hardcoded credentials, SQL injection risk and broken access control.

**Tampering** is also important because the website was defaced, but the direct `PUT /index.html` was blocked, so the successful route was probably application exploitation, account misuse, git/deployment misuse, or server-side modification.

**Spoofing, Repudiation and Elevation of Privilege** become more serious under the insider-threat model, but they still need stronger host/account evidence.

**Denial of Service** is not currently evidenced.

Overall, the threat model supports a multi-stage compromise involving internal access, application weakness, and possible account or deployment misuse, rather than a single direct HTTP PUT attack.

```

