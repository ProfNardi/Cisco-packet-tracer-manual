**Language:** [🇬🇧 English](README.md) | [🇮🇹 Italiano](README.it.md)

---

# Cisco Packet Tracer – Lab Technical Manual

Operational manual for using **Cisco Packet Tracer** aimed at studying networks and performing practical lab exercises.

The document has a **procedural approach**, practice-oriented and designed to be consulted quickly during configuration and troubleshooting activities.

---

## Manual Objectives

- Provide a clear and progressive guide to using Cisco Packet Tracer
- Minimize typical configuration errors
- Support the study of networking concepts through concrete and realistic scenarios
- Build **reproducible, documented, and verifiable** configurations
- Offer quick reference guides for the most commonly used CLI commands

---

## Requirements

- **Operating System**: Windows / macOS / Linux
- **Account**: Cisco Networking Academy (free)
- **Software**: Cisco Packet Tracer (version 8.0 or higher)
- **Basic Knowledge**: TCP/IP networking concepts, IP addressing, subnet mask

---

## Installation

1. **Registration**: create a free account on Cisco Networking Academy

   [https://www.netacad.com/cisco-packet-tracer](https://www.netacad.com/cisco-packet-tracer)

2. **Download**: download the latest version for your operating system

   [https://www.netacad.com/about-networking-academy/packet-tracer](https://www.netacad.com/about-networking-academy/packet-tracer)

3. **Installation and Launch**: follow the setup wizard and log in with your Cisco credentials

---

## Working Environment

On startup, Cisco Packet Tracer presents an interface organized into several areas:

- **Central Work Area**: space to create and view network topology
- **Device Bar**: library of network devices (routers, switches, PCs, etc.)
- **Configuration Panels**: tools to configure devices via GUI or CLI
- **Simulation Bar**: to visualize packet flow in real-time

![Cisco Packet Tracer desktop environment with empty workspace](./images/desktop.png)

---

## Manual Structure

### 📘 CLI Guide
Quick reference for essential Cisco IOS Command Line Interface (CLI) commands:
- **[CLI Guide in English](./cli/cli.md)**
- **[Guida CLI in Italiano](./cli/cli.it.md)**

### 📙 Theory Reference
Core concepts and deeper insights:
- **[IPv4 Structure, Masks and CIDR](./theory/ipv4-mask.md)** — IPv4 addresses, network/broadcast, subnetting and supernetting
- **[Private Networks and NAT](./theory/reti-private.md)** — RFC 1918 ranges, NAT and APIPA

### 🔬 Lab Scenarios

Scenarios are organized in order of **increasing complexity**, starting from simple configurations to multi-router architectures.

Each scenario follows the same structure:

1. **Network Topology**: diagram and architecture description
2. **IP Addressing Plan**: complete table of assigned addresses
3. **Device Configuration**: step-by-step instructions (GUI and CLI)
4. **Connectivity Verification**: functionality tests and troubleshooting

---

### 📁 Scenario List

#### [Scenario 1 – Single LAN](./scenario/scenario1.md)
Configuration of a simple LAN with one switch and three PCs. Ideal for understanding basic IP addressing concepts and layer 2 connectivity.

#### [Scenario 2 – Two LANs connected by a router (Forwarding)](./scenario/scenario2.md)
Configuration of two separate LANs connected via a router. Introduces gateway concepts, inter-network routing, and packet forwarding.

#### [Scenario 3 – Three LANs with static routing (Ring)](./scenario/scenario3.md)
Advanced architecture with three routers connected in a ring. Introduces static routing protocols and path redundancy.
