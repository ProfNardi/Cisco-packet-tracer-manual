**Language:** [English](ipv4-mask.md) | [Italiano](ipv4-mask.it.md)

**Home:** [README.md](../README.md)

# IPv4 Address Structure

An IPv4 address consists of **32 bits** represented in four octets:

```txt
11000000 10101000 00000001 00001011   in binary
192.168.1.11                          dotted-decimal notation
```

| Octet | Binary    | Decimal |
|-------|-----------|---------|
| 1st   | 11000000  | 192     |
| 2nd   | 10101000  | 168     |
| 3rd   | 00000001  | 1       |
| 4th   | 00001011  | 11      |

---

# Netmask

The netmask is a 32-bit IPv4 address that identifies the network portion and the host portion of an IP address:

```txt
Suppose we fix 24 bits for the network part

R = Network            H = Host
RRRRRRRR RRRRRRRR RRRRRRRR HHHHHHHH
11111111 11111111 11111111 00000000   in binary
255.255.255.0                         dotted-decimal notation
/24                                   CIDR (Classless Inter-Domain Routing) notation
```

## Address analysis 

The netmask is used to:
*  **extract the network** an IP address belongs to via a bitwise **AND** operation.
*  **compute the broadcast address**

Suppose we have a printer with IP address 192.168.1.11 and a mask of 255.255.255.0

```txt
11000000 10101000 00000001 00001011   (printer IP)
11111111 11111111 11111111 00000000   (netmask)
───────────────────────────────────   AND (bitwise AND)
11000000 10101000 00000001 00000000   (resulting network address: 192.168.1.0)
```

**Setting all host bits to 1 on the network address yields the broadcast address**. 

```txt
11000000 10101000 00000001 11111111   (resulting broadcast address: 192.168.1.255)

```
**Result**: The network is **192.168.1.0**

The broadcast address is **192.168.1.255**

## Usable addresses (Host Range)

Since the network address and the broadcast address are reserved and cannot be assigned to devices:

**Usable addresses = 2^(host bits) - 2**

In our /24 example (8 host bits):
- Total addresses: 2^8 = 256
- Minus network (192.168.1.**0**): -1
- Minus broadcast (192.168.1.**255**): -1
- **Assignable addresses: 254**

Addresses from 192.168.1.1 to 192.168.1.254 are available for hosts and devices.


# Subnetting

**Subnetting consists of splitting a network into multiple contiguous networks of equal size**.

---

## Principle

You borrow bits **from the host part** without touching the **network** bits.

Example: given 192.168.1.0/24, we borrow 3 bits from the host part.

```txt
R = Network    S = Subnet     H = Host

RRRRRRRR RRRRRRRR RRRRRRRR SSSHHHHH
```
Analyzing combinations per bit:

**8 subnets (2³) with 30 hosts each (2⁵−2)**

Each subnet has its own network and broadcast addresses:

```txt
11000000 10101000 00000001 000 00000   <- (Network of 1st subnet: 192.168.1.0)
11000000 10101000 00000001 001 00000   <- (Network of 2nd subnet: 192.168.1.32)
11000000 10101000 00000001 010 00000   <- (Network of 3rd subnet: 192.168.1.64)
11000000 10101000 00000001 011 00000   <- (Network of 4th subnet: 192.168.1.96)
11000000 10101000 00000001 100 00000   <- (Network of 5th subnet: 192.168.1.128)
11000000 10101000 00000001 101 00000   <- (Network of 6th subnet: 192.168.1.160)
11000000 10101000 00000001 110 00000   <- (Network of 7th subnet: 192.168.1.192)
11000000 10101000 00000001 111 00000   <- (Network of 8th subnet: 192.168.1.224)

11000000 10101000 00000001 000 11111   <- (Broadcast of 1st subnet: 192.168.1.31)
11000000 10101000 00000001 001 11111   <- (Broadcast of 2nd subnet: 192.168.1.63)
11000000 10101000 00000001 010 11111   <- (Broadcast of 3rd subnet: 192.168.1.95)
11000000 10101000 00000001 011 11111   <- (Broadcast of 4th subnet: 192.168.1.127)
11000000 10101000 00000001 100 11111   <- (Broadcast of 5th subnet: 192.168.1.159)
11000000 10101000 00000001 101 11111   <- (Broadcast of 6th subnet: 192.168.1.191)
11000000 10101000 00000001 110 11111   <- (Broadcast of 7th subnet: 192.168.1.223)
11000000 10101000 00000001 111 11111   <- (Broadcast of 8th subnet: 192.168.1.255)
```

# Supernetting (Aggregation)

**Supernetting consists of aggregating multiple contiguous networks of the same size into a single larger network**.

> In practice, it is the inverse operation of subnetting.

---

## Principle

You borrow bits **from the network part**, leaving the host part unchanged.

Example: aggregation of **4 contiguous /24 networks**, starting from **192.168.0.0/24**.


```txt
R = Network  X = Supernetting H = Host

RRRRRRRR RRRRRRRR RRRRRRXX HHHHHHHH
```
We obtain:

* Aggregated networks: 4 → 2²
* Bits borrowed from the network part: 2
* New prefix: /22
* Host bits: 32 − 22 = 10
* Usable hosts: 2¹⁰ − 2 = 1022

```txt
11000000 10101000 000000 00 00000000   <- Network 1: 192.168.0.0/24
11000000 10101000 000000 01 00000000   <- Network 2: 192.168.1.0/24
11000000 10101000 000000 10 00000000   <- Network 3: 192.168.2.0/24
11000000 10101000 000000 11 00000000   <- Network 4: 192.168.3.0/24

11000000 10101000 000000 00 11111111   <- Broadcast net 1: 192.168.0.255
11000000 10101000 000000 01 11111111   <- Broadcast net 2: 192.168.1.255
11000000 10101000 000000 10 11111111   <- Broadcast net 3: 192.168.2.255
11000000 10101000 000000 11 11111111   <- Broadcast net 4: 192.168.3.255
```

**Resulting supernet:**

* Network: 192.168.0.0/22
* Broadcast: 192.168.3.255
* Host range: 192.168.0.1 – 192.168.3.254
* Assignable hosts: 1022

---

Back to home: [README.md](../README.md)
