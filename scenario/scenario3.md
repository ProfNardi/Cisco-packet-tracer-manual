**Language:** [🇮🇹 Italiano](scenario3.it.md) | [🇬🇧 English](scenario3.md)

**Home:** [README.md](../README.md)

## Scenario 3 – Three PRIVATE LAN Networks with Ring-Connected Routers (Static Routing)

### Topology

This scenario features a **multi-router** architecture with path redundancy:

- **3 routers** (example: model 1941)
- **3 switches** (example: model 2960)
- **3 distinct PRIVATE LAN networks (RFC 1918)** (one for each router)
- Each LAN is connected to a switch, which in turn is connected to a router interface
- Routers are interconnected through **point-to-point serial links** (3 /30 networks)
- **Ring topology**: each router is connected to two others, providing redundancy

This configuration allows for alternative paths in case of a link failure.

---

### IP Addressing Plan

#### PRIVATE LAN Networks (User Access) RFC 1918

| LAN   | Router | LAN Network      | Gateway         | Host Range                   |
|-------|--------|------------------|-----------------|------------------------------|
| LAN 0 | R0     | 10.0.0.0/8       | 10.255.255.254  | 10.0.0.1 – 10.255.255.253    |
| LAN 1 | R1     | 172.16.0.0/12    | 172.31.255.254  | 172.16.0.1 – 172.31.255.253  |
| LAN 2 | R2     | 192.168.0.0/16   | 192.168.255.254 | 192.168.0.1 – 192.168.255.253|

![Scenario 3](../images/scenario3.png)

#### WAN Links Between Routers (Point-to-Point)

**/30 subnets** for serial links (2 usable hosts per link).

| Connection   | /30 Network   | Interface IP A    | Interface IP B    | Interfaces         |
|--------------|---------------|-------------------|-------------------|--------------------|
| R0 ↔ R1      | 200.0.0.0/30  | 200.0.0.1 (R0)    | 200.0.0.2 (R1)    | R0: s3/0, R1: s3/0 |
| R1 ↔ R2      | 200.0.0.4/30  | 200.0.0.5 (R1)    | 200.0.0.6 (R2)    | R1: s2/0, R2: s3/0 |
| R2 ↔ R0      | 200.0.0.8/30  | 200.0.0.9 (R2)    | 200.0.0.10 (R0)   | R2: s2/0, R0: s2/0 |

>Draw the diagram on paper

![Router Zoom](../images/sc3-zoom.png)

---

### Configuration via GUI

#### 1. Prepare the Complete Addressing Plan (on Paper/Notepad)

Before starting, document:
- IP addresses of all LAN interfaces (host interfaces/default gateway)
- IP addresses of all WAN links (router interfaces)
- Static routing tables for each router

#### 2. Configure Each Router's LAN Interfaces

For each router, configure the default gateway address on the FastEthernet interface connected to the local LAN switch:

Path: `Config` → `Interface` → `FastEthernet0/0`

![LAN Gateway Router 0](../images/sc3-R0-p2.png)
![LAN Gateway Router 1](../images/sc3-R1-p2.png)
![LAN Gateway Router 2](../images/sc3-R2-p2.png)

#### 3. Configure Serial WAN Interfaces

For each point-to-point link, configure both serial interfaces of the connected routers:

Path: `Config` → `Interface` → `Serial0/0/0` (or `Serial0/0/1`)

> **Important Note**: For serial links, one of the two routers must be configured as **DCE** (Data Communications Equipment) and must set the **clock rate**. Typically 64000 bps is set for lab environments.

![WAN Serial Router 0](../images/sc3-R0-ser2.png)
![WAN Serial Router 0](../images/sc3-R0-ser3.png)
![WAN Serial Router 1](../images/sc3-R1-ser2.png)
![WAN Serial Router 1](../images/sc3-R1-ser3.png)
![WAN Serial Router 2](../images/sc3-R2-ser2.png)
![WAN Serial Router 2](../images/sc3-R2-ser3.png)

#### 4. Configure Default Gateway on PCs

On each PC, set the IP address of the router in its LAN as the gateway:

Path: `Config` → `Global` → `Settings`

> See scenario 2

#### 5. Configure PC IP Addresses

Assign each PC an IP address from its LAN network:

Path: `Interface` → `FastEthernet0`

> See scenario 1 or 2

#### 6. Configure Static Routing

**Essential**: Routers must know the routes to remote networks. Use the CLI section below to configure static routes on each router.
![Routing R0](../images/sc3-R0-p6.png)
![Routing R1](../images/sc3-R1-p6.png)
![Routing R2](../images/sc3-R2-p6.png)
---

### Configuration via CLI

#### Complete Router 1 (R1) Configuration

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1

! LAN interface configuration
R1(config)# interface FastEthernet0/0
R1(config-if)# ip address 172.31.255.254 255.240.0.0
R1(config-if)# description LAN 1
R1(config-if)# no shutdown
R1(config-if)# exit

! R1–R0 link configuration (DCE serial interface)
R1(config)# interface Serial3/0
R1(config-if)# ip address 200.0.0.2 255.255.255.252
R1(config-if)# description Link to R0
R1(config-if)# clock rate 64000
R1(config-if)# no shutdown
R1(config-if)# exit

! R1–R2 link configuration (DTE serial interface)
R1(config)# interface Serial2/0
R1(config-if)# ip address 200.0.0.5 255.255.255.252
R1(config-if)# description Link to R2
R1(config-if)# no shutdown
R1(config-if)# exit

! Static route configuration to remote networks
! Route to LAN 0 (via R0)
R1(config)# ip route 10.0.0.0 255.0.0.0 200.0.0.1
! Route to LAN 2 (via R2)
R1(config)# ip route 192.168.0.0 255.255.0.0 200.0.0.6
! Route to R2-R0 link (via R2)
R1(config)# ip route 200.0.0.8 255.255.255.252 200.0.0.6

! Save configuration
R1(config)# exit
R1# write memory
```

#### Complete Router 2 (R2) Configuration

```text
Router> enable
Router# configure terminal
Router(config)# hostname R2

! LAN interface configuration
R2(config)# interface FastEthernet0/0
R2(config-if)# ip address 192.168.255.254 255.255.0.0
R2(config-if)# description LAN 2
R2(config-if)# no shutdown
R2(config-if)# exit

! R2–R1 link configuration (DCE serial interface)
R2(config)# interface Serial3/0
R2(config-if)# ip address 200.0.0.6 255.255.255.252
R2(config-if)# description Link to R1
R2(config-if)# clock rate 64000
R2(config-if)# no shutdown
R2(config-if)# exit

! R2–R0 link configuration (DTE serial interface)
R2(config)# interface Serial2/0
R2(config-if)# ip address 200.0.0.9 255.255.255.252
R2(config-if)# description Link to R0
R2(config-if)# no shutdown
R2(config-if)# exit

! Static route configuration to remote networks
! Route to LAN 1 (via R1)
R2(config)# ip route 172.16.0.0 255.240.0.0 200.0.0.5
! Route to LAN 0 (via R0)
R2(config)# ip route 10.0.0.0 255.0.0.0 200.0.0.10
! Route to R0-R1 link (via R0)
R2(config)# ip route 200.0.0.0 255.255.255.252 200.0.0.10

! Save configuration
R2(config)# exit
R2# write memory
```

#### Complete Router 0 (R0) Configuration

```text
Router> enable
Router# configure terminal
Router(config)# hostname R0

! LAN interface configuration
R0(config)# interface FastEthernet0/0
R0(config-if)# ip address 10.255.255.254 255.0.0.0
R0(config-if)# description LAN 0
R0(config-if)# no shutdown
R0(config-if)# exit

! R0–R1 link configuration (DTE serial interface)
R0(config)# interface Serial3/0
R0(config-if)# ip address 200.0.0.1 255.255.255.252
R0(config-if)# description Link to R1
R0(config-if)# no shutdown
R0(config-if)# exit

! R0–R2 link configuration (DCE serial interface)
R0(config)# interface Serial2/0
R0(config-if)# ip address 200.0.0.10 255.255.255.252
R0(config-if)# description Link to R2
R0(config-if)# clock rate 64000
R0(config-if)# no shutdown
R0(config-if)# exit

! Static route configuration to remote networks
! Route to LAN 1 (via R1)
R0(config)# ip route 172.16.0.0 255.240.0.0 200.0.0.2
! Route to LAN 2 (via R2)
R0(config)# ip route 192.168.0.0 255.255.0.0 200.0.0.9
! Route to R1-R2 link (via R1)
R0(config)# ip route 200.0.0.4 255.255.255.252 200.0.0.2

! Save configuration
R0(config)# exit
R0# write memory
```

---

### Configuration Verification

#### 1. Verify Interface Status

On each router, check that all interfaces are active:

```text
R1# show ip interface brief
```

Expected output: all configured interfaces should show **Status: up** and **Protocol: up**.

#### 2. Verify Routing Table

On each router, display the routing table:

```text
R1# show ip route
```

Verify the presence of:
- **C** (Connected) routes: directly connected networks
- **S** (Static) routes: manually configured static routes

#### 3. Basic Connectivity Test

From a PC command prompt, test:

```text
# Ping to local gateway (example from LAN 0)
ping 10.255.255.254

# Ping to a PC in another LAN (example from LAN 0 to LAN 1)
ping 172.16.0.10

# Ping to a remote router interface
ping 200.0.0.2
```

#### 4. Path Tracing

To view the path followed by packets:

```text
tracert 192.168.0.10
```

This command shows all the hops (routers) traversed by the packet.

---

### Troubleshooting

#### Problem: PC Cannot Ping a PC in Another LAN

**Possible causes:**

1. **Gateway not configured on PC**: verify that each PC has the correct gateway set
2. **Missing or incorrect static routes**: verify with `show ip route` that each router knows all networks
3. **Interfaces down**: verify with `show ip interface brief` that all interfaces are "up"
4. **Clock rate not configured**: on serial links, at least one end must be DCE with clock rate set

#### Problem: First Ping Fails, Then Works

**Cause**: Normal. The first packet is used to populate ARP tables. Subsequent packets will succeed.

#### Problem: Serial Interface Remains "down/down"

**Possible solutions:**

1. Verify that the serial cable is the correct type (DCE or DTE)
2. Verify that the clock rate is configured on the DCE side
3. Check that both ends have the `no shutdown` command
4. In Packet Tracer: verify that both routers are powered on

#### Problem: Routing Doesn't Work When a Link Fails

**Cause**: With static routing, if a link goes down, routes don't update automatically. To have automatic failover, it's necessary to implement dynamic routing (RIP, EIGRP, OSPF) or configure static routes with floating metric.

---

### Deep Dive: Static Routing

#### What is Static Routing?

**Static routing** consists of manually configuring routes on each router to networks that are not directly connected. Each static route specifies:

- **Destination network**: the remote network address
- **Subnet mask**: the mask of the remote network
- **Next hop**: the IP address of the next router to forward packets to

#### Advantages

- Complete control over packet path
- No overhead from dynamic routing protocols
- Suitable for small networks and simple topologies
- Greater security (no exchange of routing information)

#### Disadvantages

- Manual configuration required for each router
- No automatic adaptation to topology changes
- Difficult to manage in large networks
- In case of link failure, traffic is not automatically redirected

#### When to Use Static Routing

- Small networks with stable topology
- Simple point-to-point connections
- Default routes to the Internet
- Laboratory and testing scenarios

---

### Theoretical References

- Subnetting for point-to-point links (/30) and CIDR mask concepts: [theory/ipv4-mask.md](theory/ipv4-mask.md)
- Cisco IOS CLI command guide: [cli/cli.md](cli/cli.md)

---

Return to home: [README.md](../README.md)
