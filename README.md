**Language:** [🇬🇧 English](README.md) | [🇮🇹 Italiano](README.it.md)

---

# Cisco Packet Tracer – Lab Technical Manual

Operational manual for using **Cisco Packet Tracer** aimed at studying networks and performing practical lab exercises.

The document has a **procedural approach**, practice-oriented and designed to be consulted quickly during configuration and troubleshooting activities.

---

## Installation

1. **Registration**: create a free account on Cisco Networking Academy

   [https://www.netacad.com/cisco-packet-tracer](https://www.netacad.com/cisco-packet-tracer)

2. **Download**: download the latest version for your operating system

   [https://www.netacad.com/about-networking-academy/packet-tracer](https://www.netacad.com/about-networking-academy/packet-tracer)

3. **Installation and Launch**: follow the setup wizard and log in with your Cisco credentials

![Cisco Packet Tracer desktop environment with empty workspace](./images/desktop.png)
---

## Manual Structure

### 📘 CLI Guide
Quick reference for essential Cisco IOS Command Line Interface (CLI) commands:
- **[CLI Guide in English](./cli/cli.md)**

### 📙 Theory Reference
Core concepts and deeper insights:
- **[IPv4 Structure, Masks and CIDR](./theory/ipv4-mask.md)** — IPv4 addresses, network/broadcast, subnetting and supernetting
- **[Private Networks and NAT](./theory/reti-private.md)** — RFC 1918 ranges, NAT and APIPA

### 🔬 Lab Scenarios

Scenarios are organized in order of **increasing complexity**, starting from simple configurations to multi-router architectures.

#### [Scenario 1 – Single LAN](./scenario/scenario1.md)
Configuration of a simple LAN with one switch and three PCs. Ideal for understanding basic IP addressing concepts and layer 2 connectivity.

#### [Scenario 2 – Two LANs connected by a router (Forwarding)](./scenario/scenario2.md)
Configuration of two separate LANs connected via a router. Introduces gateway concepts, inter-network routing, and packet forwarding.

#### [Scenario 3 – Three PRIVATE LANs with static routing (Ring)](./scenario/scenario3.md)
Advanced architecture with three routers connected in a ring. Introduces static routing protocols and path redundancy.
