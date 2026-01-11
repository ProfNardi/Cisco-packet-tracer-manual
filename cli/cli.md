**Language:** [English](cli.md) | [Italiano](cli.it.md)

**Home:** [README.md](../README.md)

# Cisco IOS – Essential CLI Commands Manual

Practical guide for basic configuration of **Cisco switches** and **routers** via command line interface (CLI) in laboratory.

---

## Cisco IOS CLI Modes

**Cisco IOS** uses several hierarchical configuration modes:

- **User EXEC Mode** (`>`): limited mode, only basic display commands
- **Privileged EXEC Mode** (`#`): full access to display and debug commands
- **Global Configuration Mode** (`(config)#`): mode for global device configuration
- **Interface Configuration Mode** (`(config-if)#`): specific interface configuration

---

## Initial Commands

### Accessing Configuration Modes
```bash
enable                          # Switch from user mode (>) to privileged mode (#)
configure terminal              # Enter global configuration mode (config)#
```

### Basic Device Configuration
```bash
hostname Router1                # Change the device name (switch/router)
enable secret MyPassword123     # Set the password for privileged mode access
```

### Remote Access Configuration (Telnet/SSH)
```bash
line vty 0 15                   # Configure virtual lines for remote access (from 0 to 15)
password MyTelnetPass           # Set the password for remote connections
login                           # Require authentication at access
```

### Console Optimization
```bash
no ip domain-lookup             # Disable DNS resolution (avoids delays with wrong commands)
line console 0                  # Configure the console line
logging synchronous             # Synchronize log messages to not interrupt commands
exec-timeout 0 0                # Disable timeout (useful in lab, not recommended in production)
```

---

## Interface Configuration

### Router: Physical Interface Configuration
```bash
configure terminal
interface GigabitEthernet 0/0   # Select the interface to configure
ip address 192.168.1.1 255.255.255.0    # Assign IP address and subnet mask
description Office LAN          # Add an interface description
no shutdown                     # Activate the interface (by default they are off)
exit                            # Exit interface configuration mode
```

### Switch: Virtual Interface Configuration (VLAN)
```bash
interface vlan 1                # Select the virtual interface of VLAN 1
ip address 192.168.1.10 255.255.255.0   # Assign IP for switch management
no shutdown                     # Activate the VLAN interface
```

---

## Verification and Diagnostic Commands

```bash
show ip interface brief         # Display the status of all interfaces (IP, status up/down)
show interfaces                 # Complete details of all interfaces
show version                    # Information about hardware, IOS, uptime and configuration
show cdp neighbors              # List of directly connected Cisco devices
show cdp neighbors detail       # Complete details of neighboring devices (IP, model, port)
show running-config             # Currently running configuration (RAM)
show startup-config             # Configuration saved in permanent memory (NVRAM)
show vlan brief                 # List of configured VLANs (switch only)
show ip route                   # Routing table (router only)
show mac address-table          # Switch MAC address table
```

---

## VLAN Configuration (Switch)

### Create a New VLAN
```bash
configure terminal
vlan 10                         # Create VLAN with ID 10
name Administration             # Assign a descriptive name to the VLAN
exit
```

### Assign a Port to VLAN (Access Mode)
```bash
interface FastEthernet 0/5      # Select the interface to configure
switchport mode access          # Configure the port in access mode (for end devices)
switchport access vlan 10       # Assign the port to VLAN 10
description Administration PC   # Add a description
exit
```

### Configure Trunk Port (Inter-Switch Connection)
```bash
interface GigabitEthernet 0/1   # Select the interface for trunk
switchport mode trunk           # Configure the port in trunk mode (carries all VLANs)
switchport trunk allowed vlan all   # Allow all VLANs to pass (optional)
description Trunk to Switch2    # Add a description
exit
```

> **Note**: Ports in **access** mode are used to connect end devices (PCs, printers), while **trunk** ports are used to connect switches together, allowing traffic from multiple VLANs to pass through.

---

## Configuration Save and Management

```bash
write memory                    # Save configuration from RAM to NVRAM (permanent)
# Or
copy running-config startup-config   # More explicit equivalent command
```

> **Important**: Without saving the configuration, all changes will be lost when the device is restarted!

### Other Useful Commands
```bash
erase startup-config            # Delete the saved configuration
reload                          # Restart the device
show startup-config             # Display the saved configuration
show running-config             # Display the current running configuration
```

### Exit Configuration Modes
```bash
exit                            # Return to the previous level
end                             # Return directly to privileged mode (#)
Ctrl+Z                          # Shortcut to return to privileged mode
```

---

Back to home: [README.md](../README.md)