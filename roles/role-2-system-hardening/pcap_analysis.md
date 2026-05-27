# Pcap Analysis – System Hardening Lead

**File:** `/home/darren_webdev/pcaps/barmbox_capture.pcap`

## Key findings
- The IP `192.168.1.105` (source of the `PUT /index.html` attack) is active throughout the capture (01:30 – 01:37).
- ARP packets show the device joined the network at 01:30 – consistent with the DHCP lease start time.
- HTTP GET requests from this IP include:
  - `how long does insurance investigation take uk small business` (01:33)
  - `steeplejack training courses lancashire` (01:35)
- Frame 14 contains a malformed DNS packet (actually a UDP payload) that reveals `SSID=BPBPies-Staff` and `LOCATION=inside_bakery`.

## Significance
- The web searches directly support the insurance fraud motive.
- The location data proves the device was physically inside the bakery.
- The capture confirms that `192.168.1.105` was the attacker’s device and ties it to Maureen’s laptop.