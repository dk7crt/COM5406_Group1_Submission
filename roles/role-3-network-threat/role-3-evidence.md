
# Role 3 Evidence Document  
## Network and Threat Modelling Lead  
**Name:** Maduabuchi Victor Orizu  
**Case:** Bee Pies & Pasties Post-Incident Investigation  
**Module:** COM5406 Cyber Security  

---

## 1. Role Scope

My role is Network and Threat Modelling Lead. I am responsible for analysing network evidence, maintaining the STRIDE threat model, mapping the attack surface, and explaining how network activity connects to the wider incident.

My key artefacts are:

- Network evidence analysis
- STRIDE threat model
- Network topology diagram
- Attack surface map
- Attack path diagram
- PCAP analysis

A key part of my approach is separating **direct evidence** from **inference**. I do not treat an IP address, hostname, git author name, or suspected actor as proof of identity unless it is supported by other evidence.

---

## 2. Evidence Reviewed

The main evidence relevant to my role includes:

```text
192.168.1.105
PUT /index.html
405 Method Not Allowed
/home/darren_webdev/pcaps/barmbox_capture.pcap
CAPTURE_META: SSID=BPBPies-Staff SIGNAL=-42dBm LOCATION=inside_bakery
DB_PASSWORD = 'freddibnah1938'
DHCP mapping to MAUREEN-LAPTOP
````

The suspicious traffic involved the internal IP address `192.168.1.105`. Because this is a private LAN address, I treated it as evidence of an internal device or internal access path, not proof of a named person.

The `PUT /index.html` request was important because it targeted web content using a write-oriented HTTP method. However, the response was `405 Method Not Allowed`, meaning this request was likely blocked. Therefore, it is suspicious evidence of attempted tampering, but not proof that this exact request caused the defacement.

---

## 3. PCAP Analysis

The pcap analysed was:

```text
/home/darren_webdev/pcaps/barmbox_capture.pcap
```

Key facts:

```text
Packets: 30
Protocols: HTTP, DNS, ARP, TCP, UDP
Source IP: 192.168.1.105
Capture metadata: BPBPies-Staff, inside_bakery
```

The pcap showed readable HTTP traffic, which supports **Information Disclosure** because HTTP does not encrypt request details.

The metadata string was also significant:

```text
CAPTURE_META: SSID=BPBPies-Staff SIGNAL=-42dBm LOCATION=inside_bakery
```

This supports the view that the traffic was linked to the staff Wi-Fi / bakery environment. However, it still does not prove the human actor.

---

## 4. Application and Code Evidence

The investigation also identified application-level weaknesses, including SQL injection risk, broken access control, and hardcoded credentials.

The strongest code evidence was:

```text
DB_PASSWORD = 'freddibnah1938'
```

This supports **Information Disclosure** because credentials were exposed in the codebase. If valid, the password could also support **Spoofing** or **Elevation of Privilege**, depending on what access it allowed.

SQL injection and broken access control also increased the risk of customer/order data exposure and unauthorised database interaction.

---

## 5. STRIDE Summary

| STRIDE Category        | Current Position                                                                                                                                                 |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Spoofing               | Possible. `192.168.1.105` and `MAUREEN-LAPTOP` identify a device, not a confirmed person. Hardcoded credentials also increase spoofing risk.                     |
| Tampering              | Evidenced/likely. The site was defaced and `/index.html` was targeted, but the direct `PUT` returned `405`, so the successful method was probably another route. |
| Repudiation            | Possible. Attribution remains weak because IPs, hostnames and git metadata do not prove who performed the action.                                                |
| Information Disclosure | Strongest finding. Cleartext HTTP was visible in the pcap, staff Wi-Fi metadata was exposed, and the code contained a hardcoded database password.               |
| Denial of Service      | Not evidenced. The pcap has only 30 packets and shows no flood or outage pattern.                                                                                |
| Elevation of Privilege | Possible but not proven. This requires further support from auth logs, sudo logs, bash history, permissions and deployment evidence.                             |

---

## 6. Attack Surface Summary

The main attack surface areas identified were:

* Bee Pies Flask web application
* Admin/login routes
* Order form and order lookup routes
* Database queries
* Hardcoded credentials
* Staff Wi-Fi network
* Internal LAN devices
* Server user accounts
* Git/deployment process

The investigation showed that the incident should not be explained by the blocked `PUT` request alone. The more realistic model includes application weaknesses, internal network access, possible account misuse, and deployment or server-side modification.

---

## 7. Working Attack Path

```text
Internal device / staff network access
        ↓
Interaction with Bee Pies web application
        ↓
Application weakness or account misuse
        ↓
Possible server, git, or deployment change
        ↓
Website defacement
```

The weakest part of this chain is attribution. The evidence links activity to a device and network context, but not conclusively to a specific human actor.

---

## 8. Changed Understanding

My understanding changed during the investigation.

At first, the `PUT /index.html` request looked like the likely cause of the defacement. Later evidence showed that it returned `405 Method Not Allowed`, meaning it was probably blocked.

This changed my conclusion. The direct `PUT` request is still suspicious, but the successful tampering likely happened through another route, such as SQL injection, broken access control, git/deployment misuse, account misuse, or server-side modification.

The strongest evidenced STRIDE category is now **Information Disclosure**, because the pcap showed readable HTTP traffic and the application contained exposed credentials and data exposure risks.

---

## 9. Residual Risk

Several risks remain after the investigation:

* Attribution is still weak because device evidence does not prove the human actor.
* Cleartext HTTP remains a risk unless HTTPS/TLS is enforced.
* Hardcoded credentials must be removed and rotated.
* Broken access control must be fully fixed.
* SQL injection patterns must be removed across the application.
* The deployment path remains unclear, so repository or server-side modification cannot be ruled out.
* Further evidence is needed from auth logs, sudo logs, bash history, permissions and git history.

---

## 10. Final Conclusion

By the Week 4B stage, my role evidence supports a stronger and more balanced threat model.

The evidence suggests internal network relevance through `192.168.1.105`, `MAUREEN-LAPTOP`, and the staff Wi-Fi metadata. However, this does not prove the human actor.

The blocked `PUT /index.html` request supports attempted tampering, but not the final defacement mechanism. The strongest confirmed threat is **Information Disclosure**, supported by readable HTTP traffic, exposed staff Wi-Fi context, hardcoded credentials, and application weaknesses.

Overall, the incident is best explained as a multi-stage compromise involving internal network access, application weaknesses, and possible account or deployment misuse, rather than a single direct HTTP `PUT` attack.

```

