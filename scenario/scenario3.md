**Language:** [English](scenario3.md) | [Italiano](scenario3.it.md)

**Home:** [README.md](../README.md)

## Scenario 3 – Three LANs with routers in a ring (Static Routing)

### Topology

This scenario features a **multi-router** architecture with path redundancy:

- **3 routers** (example: model 1941)
- **3 switches** (example: model 2960)
- **3 distinct LANs** (one per router)
- Each LAN connects to a switch, which in turn connects to a router interface
- Routers are interconnected via **point-to-point** serial links (3 /30 networks)
- **Ring topology**: each router connects to two others, providing redundancy

This configuration allows alternative paths if a link fails.

![Scenario 3](../images/scenario3.png)

---

### IP Addressing Plan

#### LANs (user access)

| LAN   | Router | LAN Network      | Gateway     | Usable Hosts       |
|-------|--------|------------------|-------------|--------------------|
| LAN 1 | R1     | 192.168.1.0/24   | 192.168.1.1 | 192.168.1.2–254    |
| LAN 2 | R2     | 192.168.2.0/24   | 192.168.2.1 | 192.168.2.2–254    |
| LAN 3 | R3     | 192.168.3.0/24   | 192.168.3.1 | 192.168.3.2–254    |

#### WAN point-to-point links between routers

Use **/30** subnets for serial links (2 usable hosts per link).

| Link      | /30 Network    | Router A IP     | Router B IP     | Interfaces             |
|-----------|----------------|-----------------|-----------------|------------------------|
| R1 ↔ R2   | 10.0.0.0/30    | 10.0.0.1 (R1)   | 10.0.0.2 (R2)   | R1: s0/0/0, R2: s0/0/0 |
| R2 ↔ R3   | 10.0.0.4/30    | 10.0.0.5 (R2)   | 10.0.0.6 (R3)   | R2: s0/0/1, R3: s0/0/0 |
| R3 ↔ R1   | 10.0.0.8/30    | 10.0.0.9 (R3)   | 10.0.0.10 (R1)  | R3: s0/0/1, R1: s0/0/1 |

---

### Configuration via GUI

#### 1. Prepare the complete addressing plan

Before starting, document:
- IP addresses of all LAN interfaces
- IP addresses of all WAN links
- Static routing tables for each router

#### 2. Configure each router's LAN interface

On each router, configure the GigabitEthernet interface connected to the local LAN switch:

Path: `Config` → `Interface` → `GigabitEthernet0/0`

![Router LAN Interface](../images/sc3-rou-lan-int.png)

#### 3. Configure serial WAN interfaces

For each point-to-point link, configure both routers' serial interfaces:

Path: `Config` → `Interface` → `Serial0/0/0` (or `Serial0/0/1`)

> **Important**: For serial links, one router must be configured as **DCE** (Data Communications Equipment) and set a **clock rate**. In labs, 64000 bps is commonly used.

![Serial interface configuration](../images/sc3-rou-wan-int.png)

#### 4. Configure default gateway on PCs

On each PC, set the router IP of its LAN as the default gateway:

Path: `Config` → `Global` → `Settings`

![PC Gateway](../images/sc3-pc-gateway.png)

#### 5. Configure PCs' IP addresses

Assign an IP address within the LAN to each PC:

Path: `Interface` → `FastEthernet0`

![PC IP](../images/sc3-pc-ip.png)

#### 6. Configure static routing

**Essential**: Routers must know routes to remote networks. Use the CLI section below to configure static routes on each router.

---

### Configuration via CLI

#### Complete configuration – Router 1 (R1)

```text
Router> enable
Router# configure terminal
Router(config)# hostname R1

! LAN interface
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# description LAN 1
R1(config-if)# no shutdown
R1(config-if)# exit

! Link R1–R2 (DCE serial interface)
R1(config)# interface Serial0/0/0
R1(config-if)# ip address 10.0.0.1 255.255.255.252
R1(config-if)# description Link to R2
R1(config-if)# clock rate 64000
R1(config-if)# no shutdown
R1(config-if)# exit

! Link R1–R3 (DTE serial interface)
R1(config)# interface Serial0/0/1
R1(config-if)# ip address 10.0.0.10 255.255.255.252
R1(config-if)# description Link to R3
R1(config-if)# no shutdown
R1(config-if)# exit

! Static routes to remote networks
! Route to LAN 2 (via R2)
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
! Route to LAN 3 (via R3)
R1(config)# ip route 192.168.3.0 255.255.255.0 10.0.0.9
! Route to link R2–R3 (via R2)
R1(config)# ip route 10.0.0.4 255.255.255.252 10.0.0.2

! Save configuration
R1(config)# exit
R1# write memory
```

#### Complete configuration – Router 2 (R2)

```text
Router> enable
Router# configure terminal
Router(config)# hostname R2

! LAN interface
R2(config)# interface GigabitEthernet0/0
R2(config-if)# ip address 192.168.2.1 255.255.255.0
R2(config-if)# description LAN 2
R2(config-if)# no shutdown
R2(config-if)# exit

! Link R2–R1 (DTE serial interface)
R2(config)# interface Serial0/0/0
R2(config-if)# ip address 10.0.0.2 255.255.255.252
R2(config-if)# description Link to R1
R2(config-if)# no shutdown
R2(config-if)# exit

! Link R2–R3 (DCE serial interface)
R2(config)# interface Serial0/0/1
R2(config-if)# ip address 10.0.0.5 255.255.255.252
R2(config-if)# description Link to R3
R2(config-if)# clock rate 64000
R2(config-if)# no shutdown
R2(config-if)# exit

! Static routes to remote networks
! Route to LAN 1 (via R1)
R2(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
! Route to LAN 3 (via R3)
R2(config)# ip route 192.168.3.0 255.255.255.0 10.0.0.6
! Route to link R3–R1 (via R3)
R2(config)# ip route 10.0.0.8 255.255.255.252 10.0.0.6

! Save configuration
R2(config)# exit
R2# write memory
```

#### Complete configuration – Router 3 (R3)

```text
Router> enable
Router# configure terminal
Router(config)# hostname R3

! LAN interface
R3(config)# interface GigabitEthernet0/0
R3(config-if)# ip address 192.168.3.1 255.255.255.0
R3(config-if)# description LAN 3
R3(config-if)# no shutdown
R3(config-if)# exit

! Link R3–R2 (DTE serial interface)
R3(config)# interface Serial0/0/0
R3(config-if)# ip address 10.0.0.6 255.255.255.252
R3(config-if)# description Link to R2
R3(config-if)# no shutdown
R3(config-if)# exit

! Link R3–R1 (DCE serial interface)
R3(config)# interface Serial0/0/1
R3(config-if)# ip address 10.0.0.9 255.255.255.252
R3(config-if)# description Link to R1
R3(config-if)# clock rate 64000
R3(config-if)# no shutdown
R3(config-if)# exit

! Static routes to remote networks
! Route to LAN 1 (via R1)
R3(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.10
! Route to LAN 2 (via R2)
R3(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.5
! Route to link R1–R2 (via R1)
R3(config)# ip route 10.0.0.0 255.255.255.252 10.0.0.10

! Save configuration
R3(config)# exit
R3# write memory
```

---

### Configuration Verification

#### 1. Check interface status

On each router, verify that all interfaces are up:

```text
R1# show ip interface brief
```

Expected output: all configured interfaces should show **Status: up** and **Protocol: up**.

#### 2. Check the routing table

On each router, display the routing table:

```text
R1# show ip route
```

Verify presence of:
- **C** (Connected) routes: directly connected networks
- **S** (Static) routes: manually configured static routes

#### 3. Basic connectivity tests

From a PC command prompt, test:

```text
# Ping the local gateway
ping 192.168.1.1

# Ping a PC in another LAN
ping 192.168.2.10

# Ping a remote router interface
ping 10.0.0.2
```

#### 4. Path tracing

To view the path packets take:

```text
tracert 192.168.3.10
```

This shows all hops (routers) traversed by the packet.

---

### Troubleshooting

#### Problem: A PC cannot ping a PC in another LAN

**Possible causes:**

1. **Gateway not configured on the PC**: ensure each PC has the correct gateway set
2. **Static routes missing or wrong**: use `show ip route` to confirm each router knows all networks
3. **Interfaces down**: use `show ip interface brief` to check all interfaces are "up"
4. **Clock rate not set**: on serial links, the DCE side must set a clock rate

#### Problem: First ping fails, then it works

**Cause**: Normal. The first packet populates ARP tables; subsequent packets succeed.

#### Problem: Serial interface stays "down/down"

**Possible solutions:**

1. Check that the serial cable type is correct (DCE or DTE)
2. Ensure the clock rate is set on the DCE side
3. Confirm both ends have `no shutdown`
4. In Packet Tracer: verify both routers are powered on

#### Problem: Routing does not work when a link fails

**Cause**: With static routing, routes do not update automatically on link failure. For automatic failover, use dynamic routing (RIP, EIGRP, OSPF) or configure floating static routes with higher administrative distance.

---

### Deep Dive: Static Routing

#### What is Static Routing?

**Static routing** means manually configuring routes on each router to reach non-directly connected networks. Each static route specifies:

- **Destination network**: the remote network address
- **Subnet mask**: the mask of the remote network
- **Next hop**: the IP address of the next router to forward packets to

#### Advantages

- Full control over packet paths
- No overhead from dynamic routing protocols
- Suitable for small networks and simple topologies
- Increased security (no routing information exchanges)

#### Disadvantages

- Manual configuration required on each router
- No automatic adaptation to topology changes
- Hard to manage in large networks
- No automatic rerouting on link failure

#### When to use Static Routing

- Small networks with stable topology
- Simple point-to-point links
- Default routes to the Internet
- Lab and testing scenarios

---

### Theory References

- Subnetting for point-to-point links (/30) and CIDR mask concepts: [theory/ipv4-mask.md](../theory/ipv4-mask.md)
- Cisco IOS CLI commands guide: [cli/cli.md](../cli/cli.md)

---

Back to home: [README.md](../README.md)
