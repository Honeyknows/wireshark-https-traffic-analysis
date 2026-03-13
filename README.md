# 🦈 Wireshark HTTPS Traffic Analysis

Capturing and analyzing a live HTTPS connection to Google — packet by packet — on Kali Linux.

---

## What This Covers

- DNS resolution (A and AAAA records)
- TCP 3-way handshake
- TLS 1.3 negotiation (Client Hello, Server Hello, Application Data)
- Full protocol stack inspection — Ethernet → IP → UDP/TCP → DNS/TLS

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Visual packet capture and protocol analysis |
| tcpdump | Command-line capture, saves to .pcap |
| nslookup | Triggers DNS resolution |
| curl | Generates real HTTPS traffic |

---

## Capture Commands

```bash
# Start capturing HTTPS traffic
sudo tcpdump -i eth0 port 443 -w tls_capture.pcap

# Trigger DNS + HTTPS traffic
nslookup google.com
curl https://google.com
```

---

## Wireshark Filters Used

```
dns                  → View DNS queries and responses
tcp.stream eq 0      → Isolate the first TCP connection (3-way handshake)
tls.stream eq 0      → Isolate the TLS handshake
```

---

## Files

| File | Description |
|------|-------------|
| `dns.pcapng` | DNS query and response capture |
| `allinone.pcapng` | Full capture — DNS + TCP + TLS in one file |
| `tls_capture.pcap` | HTTPS traffic on port 443 |

---

## How It Works

```
Browser/curl
     │
     ▼
[1] DNS Query ──────► DNS Server resolves google.com → IP
     │
     ▼
[2] TCP Handshake ──► SYN → SYN-ACK → ACK
     │
     ▼
[3] TLS 1.3 ────────► Client Hello (SNI=google.com) → Server Hello → Encrypted
     │
     ▼
[4] Application Data (fully encrypted HTTPS traffic)
```

---

## Screenshots

See `/screenshots` folder or the PDF guide for full step-by-step breakdown with annotated captures.

---

## Environment

- OS: Kali Linux
- Interface: eth0
- Wireshark version: latest

---

*Part of my network security learning journey. Full writeup PDF in the repo.*
