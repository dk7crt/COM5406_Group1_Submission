
# Attack Path Diagram  
## Bee Pies & Pasties — Role 3 Network and Threat Modelling

**Name:** Maduabuchi Victor Orizu  
**Module:** COM5406 Cyber Security  

---

## 1. Purpose

This document maps the most likely attack path for the Bee Pies & Pasties incident using PCAP evidence, DHCP records, host findings, application vulnerabilities and STRIDE analysis.

It separates what is **evidenced** from what is **inferred**, especially around attribution and the final defacement mechanism.

---

## 2. High-Level Attack Path

```text
[1. Internal Device on Staff Wi-Fi]
 IP: 192.168.1.105
 Device: MAUREEN-LAPTOP
 SSID: BPBPies-Staff
 Location: inside_bakery
 Evidence: PCAP + DHCP + CAPTURE_META

        ↓

[2. Interaction with Bee Pies Server]
 Target: BAKERY-SERVER
 IP: 192.168.1.10
 Evidence: HTTP/DNS activity and internal network communication

        ↓

[3. Attempted Tampering]
 Request: PUT /index.html
 Response: 405 Method Not Allowed
 Meaning: Suspicious attempt, but likely blocked

        ↓

[4. Alternative Compromise Route]
 Possible routes:
 - SQL injection
 - Broken access control
 - Hardcoded DB password misuse
 - Account misuse
 - Server-side file modification
 - Git/deployment misuse

        ↓

[5. Website Defacement]
 Outcome: Website content was modified
 Conclusion: Tampering occurred, but probably not through the blocked PUT request
````

---

## 3. Attack Path Explanation

### Step 1 — Internal Device on Staff Wi-Fi

Evidence links `192.168.1.105` to `MAUREEN-LAPTOP` through DHCP records. PCAP metadata also shows:

```text
SSID=BPBPies-Staff
LOCATION=inside_bakery
```

This supports an internal-access model. However, it is only **device attribution**, not proof of the human actor.

---

### Step 2 — Interaction with Bee Pies Server

The device communicated within the Bee Pies network environment, including interaction with `BAKERY-SERVER` at `192.168.1.10`.

The PCAP also shows cleartext HTTP traffic, which supports **Information Disclosure** because request details were readable.

---

### Step 3 — Attempted Tampering

The suspicious request was:

```text
PUT /index.html
```

However, the server responded:

```text
405 Method Not Allowed
```

This means the direct `PUT` request was likely blocked. It supports **attempted Tampering**, but it does not prove that this request caused the defacement.

---

### Step 4 — Alternative Compromise Route

Because the direct `PUT` was blocked, the successful defacement likely happened through another route.

The most plausible routes are:

* SQL injection
* Broken access control
* Hardcoded database password misuse
* Account misuse
* Git/deployment misuse
* Server-side file modification

This stage links mainly to **Tampering**, **Repudiation**, and possible **Elevation of Privilege**.

---

### Step 5 — Website Defacement

The final outcome was website defacement. Since the `PUT /index.html` request returned `405`, the direct PUT should be treated as suspicious evidence, not the confirmed cause.

The stronger conclusion is that tampering occurred through an application, account, server-side, or deployment route that still needs correlation with host and git evidence.

---

## 4. Evidence Strength

| Attack Path Step                    | Strength  | Reason                                          |
| ----------------------------------- | --------- | ----------------------------------------------- |
| `192.168.1.105` active on LAN       | Strong    | Supported by PCAP/network evidence.             |
| Mapping to `MAUREEN-LAPTOP`         | Strong    | DHCP mapping supports device attribution.       |
| Staff Wi-Fi / inside bakery context | Strong    | PCAP metadata shows SSID and location.          |
| Human attribution                   | Weak      | Device evidence does not prove who used it.     |
| `PUT /index.html` attempt           | Moderate  | Suspicious request is evidenced, but blocked.   |
| `PUT` caused defacement             | Weak      | `405 Method Not Allowed` suggests it failed.    |
| Alternative compromise route        | Plausible | Needed to explain defacement after blocked PUT. |
| Website defacement                  | Strong    | Confirmed incident outcome.                     |

---

## 5. Trust Boundaries Crossed

| Boundary                                              | Relevance                                                  |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| Staff Wi-Fi → Internal LAN                            | Shows internal access from the bakery network environment. |
| Internal LAN → BAKERY-SERVER                          | Shows access toward the key application/server target.     |
| Application/account → File system or deployment layer | Inferred route needed to explain defacement.               |

---

## 6. Limitations

* The PCAP contains only 30 packets.
* It does not show the full attack sequence.
* The `PUT /index.html` request was blocked.
* There is no direct proof that SQL injection or git/deployment misuse caused the defacement.
* `MAUREEN-LAPTOP` supports device attribution, not human attribution.
* Stronger confirmation would require auth logs, bash history, git history, file timestamps and deployment evidence.

---

## 7. Conclusion

The most likely attack path is:

```text
Internal staff-network device
        ↓
Interaction with Bee Pies server/application
        ↓
Attempted direct tampering via PUT, blocked by 405
        ↓
Alternative application/account/deployment route
        ↓
Website defacement
```

This attack path supports the wider STRIDE model. **Information Disclosure** is strongly evidenced by cleartext HTTP, while **Tampering** is confirmed by the defacement but not fully explained by the blocked `PUT` request.

Overall, the evidence supports a multi-stage internal-access model, while human attribution and the exact defacement mechanism remain unproven.

```
