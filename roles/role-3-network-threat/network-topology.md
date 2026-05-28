
---

## network-topology.md 
### Bee Pies & Pasties — Network Topology 

## **1. Purpose**

This document describes the network topology relevant to the Bee Pies & Pasties incident investigation.  
It maps:

- The Bee Pies LAN (192.168.1.0/24)  
- The devices identified in DHCP evidence  
- The server hosting the Flask application  
- Trust boundaries  
- The separation between the **lab VM network** and the **real bakery LAN**  

This topology supports the STRIDE model, attack surface mapping, and incident narrative.

---

## **2. High-Level Network Overview**

The Bee Pies environment consists of two distinct networks:

### **A. Lab / VM Access Network (Used by Investigators)**  
```
192.168.56.0/24
```

Used only for investigation access:

- `192.168.56.101` — Bee Pies Flask application (lab environment)  
- Accessed via browser:  
  - `http://192.168.56.101` [(192.168.56.101 in Bing)](https://www.bing.com/search?q="http%3A%2F%2F192.168.56.101%2F")  
  - `http://192.168.56.101:5000` [(192.168.56.101 in Bing)](https://www.bing.com/search?q="http%3A%2F%2F192.168.56.101%3A5000%2F")  

**Important:**  
This network is **not** part of the real bakery LAN.  
It must not be confused with the production environment.

---

### **B. Bee Pies Production LAN (Real Environment)**  
```
192.168.1.0/24
Router: 192.168.1.1
```

This is the network where the incident occurred.

Devices identified:

| Device | IP Address | MAC | Notes |
|-------|------------|------|-------|
| **BAKERY-SERVER** | 192.168.1.10 | 00:1a:79:8e:5d:33 | Static; hosts the Bee Pies application |
| **MAUREEN-LAPTOP** | 192.168.1.105 | 8c:8d:28:a4:2f:11 | DHCP lease overlaps incident window |
| **IPAD-COUNTER** | 192.168.1.104 | a0:ce:c8:11:44:f2 | POS/ordering device |
| **EPSON-TILL-POS** | 192.168.1.103 | 00:26:ab:2b:9c:01 | Till system |
| **DARREN-WORK-PC** | 192.168.1.102 | b8:27:eb:3f:1a:09 | Developer workstation (expired lease) |

Wi‑Fi metadata from the pcap:

```
SSID = BPBPies-Staff
SIGNAL = -42 dBm
LOCATION = inside_bakery
```

This confirms the capture occurred on the **staff Wi‑Fi**, inside the bakery premises.

---

## **3. Network Topology Diagram (Text-Based)**

```
                          [Internet]
                              |
                              |
                     ---------------------
                     |   Bee Pies Router |
                     |     192.168.1.1   |
                     ---------------------
                              |
        -------------------------------------------------
        |                 |                |            |
        |                 |                |            |
 [BAKERY-SERVER]   [EPSON-TILL-POS]   [IPAD-COUNTER]   [MAUREEN-LAPTOP]
 192.168.1.10        192.168.1.103      192.168.1.104     192.168.1.105
  (Static)              (Active)            (Active)         (Expired lease)
        |
        |
   Bee Pies Flask Application
   (Production environment)
```

**Trust Boundary:**  
The Bee Pies LAN is a **trusted internal network**, but devices on it (e.g., MAUREEN-LAPTOP) can still be compromised or misused.

---

## **4. Trust Boundaries**

### **Boundary 1 — External Internet → Bee Pies Router**
- Controls inbound/outbound traffic  
- Not directly part of the incident evidence  

### **Boundary 2 — Router → Internal LAN (192.168.1.0/24)**
- All bakery devices operate here  
- Incident traffic originated inside this boundary  

### **Boundary 3 — Staff Wi‑Fi → Internal LAN**
- Metadata shows the attacker was connected to **BPBPies-Staff**  
- This boundary is important because Wi‑Fi security affects threat exposure  

### **Boundary 4 — Internal LAN → BAKERY-SERVER**
- The server hosts the Flask application  
- All suspicious HTTP activity targeted this server  

---

## **5. Application Hosting Context**

The Bee Pies Flask application runs on:

```
BAKERY-SERVER
IP: 192.168.1.10
```

This server is reachable from:

- Staff Wi‑Fi  
- Internal LAN devices  
- Possibly POS devices  

This means any compromised internal device could attack the application.

---

## **6. Network Evidence Supporting This Topology**

### **A. PCAP Evidence**
- Source IP: `192.168.1.105`  
- Cleartext HTTP traffic  
- DNS queries  
- Staff Wi‑Fi metadata  

### **B. DHCP Evidence**
Maps:

```
192.168.1.105 → MAUREEN-LAPTOP
Lease: 01:30–02:45 (incident window)
```

### **C. Host Evidence**
- Local accounts: root, ubuntu, darren_webdev, maureen  
- Permissions and group membership  
- No sudo access for darren_webdev  

---

## **7. Interpretation**

- The attacker was operating **inside the Bee Pies LAN**, not externally.  
- The activity came from a device connected to **BPBPies-Staff Wi‑Fi**.  
- The device was **MAUREEN-LAPTOP**, but this does **not** prove the human user.  
- The target was **BAKERY-SERVER (192.168.1.10)**.  
- The suspicious HTTP request (`PUT /index.html`) and cleartext browsing traffic support the STRIDE model.

---

## **8. Limitations**

- The topology is based on DHCP + pcap + host evidence; more devices may exist.  
- The pcap is small (30 packets), so not all network activity is visible.  
- Device attribution ≠ human attribution.  
- No firewall or router configuration was provided.  

---

## **9. Conclusion**

This topology accurately represents the Bee Pies production network as evidenced by DHCP leases, PCAP metadata, and host information.  
It supports the STRIDE model, attack surface mapping, and the incident narrative by showing:

- Where the suspicious traffic originated  
- How it reached the server  
- Which trust boundaries were crossed  
- Why Information Disclosure and Tampering are key threats  

---


