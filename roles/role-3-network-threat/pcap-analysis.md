
# PCAP Analysis

## PCAP Timeline Summary

I used the following command to review the packet timeline:

```bash
tshark -r pcaps/barmbox_capture.pcap -t ad
````

The capture contains **30 packets** between:

```text
2025-11-06 01:30:00
2025-11-06 01:37:08
```

## Key Observations

* Host `192.168.1.105` communicates with the router `192.168.1.1` and internal server `192.168.1.10`.
* DNS traffic is visible, including lookups for:

  * `gmail.com`
  * `www.google.com`
  * `accounts.google.com`
* HTTP traffic is visible in cleartext over port 80.
* The HTTP requests include:

```text
GET /mail HTTP/1.1
GET /?q=how+long+does+insurance+investigation+take+uk+small+business HTTP/1.1
GET /?q=steeplejack+training+courses+lancashire HTTP/1.1
```

* One malformed DNS packet appears at packet 14.

## Security Relevance

The strongest evidence in this pcap is the **cleartext HTTP traffic**. Because the HTTP requests are readable in the capture, this supports the STRIDE category of **Information Disclosure**.

The traffic also shows that `192.168.1.105` was active on the network during the capture window. However, this only supports **device/network attribution**. It does **not** prove which human user was operating the device.

The malformed DNS packet can be noted as an anomaly, but there is not enough evidence from this small capture alone to claim that it caused or proved a successful attack.

## Conclusion

This pcap provides useful network evidence, but it is limited because it contains only 30 packets. Its strongest value is showing cleartext HTTP traffic and confirming activity from `192.168.1.105` during the capture window.

Overall, the pcap supports **Information Disclosure** strongly, while attribution, tampering, and privilege escalation still require correlation with DHCP records, server logs, bash history, authentication logs and git/deployment evidence.

```


