**Language:** [English](reti-private.md) | [Italiano](reti-private.it.md)

**Home:** [README.md](../README.md)

# Private IPv4 Networks

Private networks use IP address ranges reserved by RFC 1918. These addresses are not routable on the public Internet and are translated to public addresses via NAT, allowing multiple internal devices to access the Internet while maintaining security and simplified management.

## RFC 1918 Ranges

```txt
Class A: 10.0.0.0 - 10.255.255.255 (10/8)
Class B: 172.16.0.0 - 172.31.255.255 (172.16/12)
Class C: 192.168.0.0 - 192.168.255.255 (192.168/16)
```

### How it works

- **Not publicly routable**: reserved for internal networks, not routable on the Internet
- **NAT (Network Address Translation)**: translates private internal addresses to a single public IP for external communication
- **Security**: provides a protective barrier by hiding the internal topology

### Additional private addressing

- **APIPA (169.254.0.0/16)**: automatic address assignment when DHCP is unavailable (RFC 3330)

---

Back to home: [README.md](../README.md)
