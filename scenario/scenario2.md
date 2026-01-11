**Language:** [English](scenario2.md) | [Italiano](scenario2.it.md)

**Home:** [README.md](../README.md)

## Scenario 2 – Two LANs connected by a router (Forwarding)

### Topology

- 1 router (**example: model 1941**)
- 2 switches (**example: model 2960**)
- 2 distinct LANs (each connected to one router interface)

### IP Addressing

**LAN 1**
- Network: 192.168.1.0/24  
- Gateway: 192.168.1.254  

**LAN 2**
- Network: 192.168.2.0/24 
- Gateway: 192.168.2.254 

![Scenario 2](../images/scenario2.png)

### Configuration via GUI

1. **Prepare the addressing plan**: plan all IP addresses, gateways and subnet masks in advance. Work in Cisco Packet Tracer should be reduced to pure data entry.

2. **Configure the router interfaces**: assign to each router interface the gateway IP address of its respective LAN.
   - For LAN 1: assign 192.168.1.254 to interface GigabitEthernet0/0
   - For LAN 2: assign 192.168.2.254 to interface GigabitEthernet0/1
   - Remember to turn interfaces on ("On" button)

![Interface 1](../images/sc2-rou-int1.png)
![Interface 2](../images/sc2-rou-int2.png)

3. **Configure the default gateway on each PC**: specify the router's IP address to enable communication between the two LANs.
   
   Path: `Config` → `Global` → `Settings`

![Gateway](../images/sc2-pc-gateway.png)

4. **Configure the IP address of each PC's network card**: assign a unique IP address within its own network.
   
   Path: `Interface` → `FastEthernet0`

![NIC IP](../images/sc2-pc-nic.png)

### Troubleshooting

* **Routing table not updated**: try sending a Simple PDU 3 times to update the routing table. The first communication may fail until the router learns the necessary paths.
* **Port labels not visible**: check interface labels via `Options` → `Preferences` → `Interface` → select "Always Show Port Labels in Logical Workspace".
* **Disabled interfaces**: ensure all router interfaces are turned on ("On"). Powered-off interfaces commonly cause lack of connectivity.

### Configuration via Router CLI (example)

```text
Router> enable
Router# configure terminal
Router(config)# interface g0/0
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface g0/1
Router(config-if)# ip address 192.168.2.254 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write memory
```

**Command explanations:**
- `enable`: access privileged mode
- `configure terminal`: enter global configuration mode
- `interface g0/0` (or `g0/1`): select the interface to configure
- `ip address [IP] [MASK]`: assign IP address and subnet mask to the interface
- `no shutdown`: enable the interface (interfaces are down by default)
- `exit`: exit the current configuration level
- `write memory`: save the configuration to persistent memory (NVRAM)

---

### Theory References

- IPv4 addressing, netmasks and CIDR (/24): [theory/ipv4-mask.md](../theory/ipv4-mask.md)
- Private IPv4 networks (RFC 1918): [theory/reti-private.md](../theory/reti-private.md)
- Cisco IOS CLI commands guide: [cli/cli.md](../cli/cli.md)

---

Back to home: [README.md](../README.md)
