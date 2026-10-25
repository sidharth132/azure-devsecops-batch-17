# Class Notes: How the Internet Works: DNS, Routing & TCP Handshake 🌍

**Date:** 22-02-2026  
**Topic:** Global Internet Architecture, IANA, DNS Resolution Hierarchy, and Packet Routing

---

## 📌 1. The Global Internet Ecosystem

The internet is a global network of interconnected Autonomous Systems (AS) managed by regional internet registries overseen by **IANA** (Internet Assigned Numbers Authority):

```
 [ User Browser ] -----> Local Router -----> ISP Gateway -----> Regional Internet Exchange (IXP)
                                                                            |
                                                                            v
                                                               [ Submarine Fiber Cables ]
                                                                            |
                                                                            v
                                                                 [ Remote Server Data Center ]
```

---

## 🌐 2. How DNS Resolution Works

When you enter `https://www.devopsinsiders.com` in your browser:

```
 Browser Cache -> OS Cache -> Resolving DNS Server (ISP/8.8.8.8)
                                    |
                                    v
                           [ Root DNS Server (.) ]
                                    |
                                    v
                           [ TLD DNS Server (.com) ]
                                    |
                                    v
                   [ Authoritative DNS Server (devopsinsiders.com) ]
                                    |
                           (Returns IP: 20.124.45.226)
```

1. **Browser Cache & OS Cache:** Checked first for existing cached mappings.
2. **Recursive Resolver:** Queries the root name server if not found locally.
3. **Root Name Server (`.`):** Directs query to the appropriate Top-Level Domain (TLD) server (e.g., `.com`, `.org`, `.net`).
4. **TLD Server:** Directs query to the authoritative DNS name server managing the domain.
5. **Authoritative Name Server:** Returns the exact A/AAAA record IP address.

---

## 🤝 3. TCP 3-Way Handshake

Before HTTP data is transmitted, a reliable TCP connection is established:
1. **SYN:** Client sends Synchronize packet to server port (e.g., Port 443).
2. **SYN-ACK:** Server acknowledges and sends its own SYN packet.
3. **ACK:** Client acknowledges server's SYN; connection is established.

---

## 💡 Key Takeaways
1. Understand DNS record types: `A` (IPv4), `AAAA` (IPv6), `CNAME` (Canonical alias), `MX` (Mail).
2. HTTPS adds TLS handshake on top of TCP to negotiate symmetric encryption keys.
