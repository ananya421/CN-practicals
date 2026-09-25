# Cisco Packet Tracer Practical – Configuring RIPv2 (Classless Routing)

## Aim

To configure **RIPv2 (Routing Information Protocol version 2)** in Cisco Packet Tracer using a three-router topology, VLSM addressing, and `no auto-summary`, and then verify routing operation and connectivity.

## Practical Overview

This practical uses:

- **3 Routers:** R0, R1, R2
- **3 Switches:** S0, S1, S2
- **6 PCs:** PC0–PC5
- **RIPv2** as the dynamic routing protocol
- **VLSM** with `/26` LAN subnets and `/30` WAN links
- Major network: `192.168.20.0/24`

The key difference from RIPv1 is that **RIPv2 is classless and carries subnet-mask information in routing updates**, allowing VLSM to be used.

---

## 1. Network Topology

The practical topology consists of three routers connected in a linear series. Each router connects to a switch, and each switch connects to two PCs.

![Network Topology](./screenshots/network-topology.png)

### Devices

| Device | Quantity |
|---|---:|
| Router-PT-Empty | 3 |
| 2960 Switch | 3 |
| PCs | 6 |

### Connections

| From | To | Interface |
|---|---|---|
| PC0 | S0 | Fa0/1 |
| PC1 | S0 | Fa0/2 |
| S0 | R0 | Fa0/24 → Fa2/0 |
| PC2 | S1 | Fa0/1 |
| PC3 | S1 | Fa0/2 |
| S1 | R1 | Fa0/24 → Fa2/0 |
| PC4 | S2 | Fa0/1 |
| PC5 | S2 | Fa0/2 |
| R0 | R1 | Se0/0 ↔ Se1/0 |
| R1 | R2 | Se0/0 ↔ Se1/0 |

---

## 2. VLSM IP Addressing Scheme

The `192.168.20.0/24` network is divided into different subnet sizes.

| Subnet | Purpose | Mask |
|---|---|---|
| `192.168.20.0/26` | LAN 1 – R0 | `255.255.255.192` |
| `192.168.20.64/26` | LAN 2 – R1 | `255.255.255.192` |
| `192.168.20.128/26` | LAN 3 – R2 | `255.255.255.192` |
| `192.168.20.192/30` | R0 ↔ R1 | `255.255.255.252` |
| `192.168.20.196/30` | R1 ↔ R2 | `255.255.255.252` |

### PC Configuration

| PC | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | `192.168.20.10` | `255.255.255.192` | `192.168.20.1` |
| PC1 | `192.168.20.11` | `255.255.255.192` | `192.168.20.1` |
| PC2 | `192.168.20.70` | `255.255.255.192` | `192.168.20.65` |
| PC3 | `192.168.20.71` | `255.255.255.192` | `192.168.20.65` |
| PC4 | `192.168.20.140` | `255.255.255.192` | `192.168.20.129` |
| PC5 | `192.168.20.141` | `255.255.255.192` | `192.168.20.129` |

---

## 3. Router Configuration

### R0 Configuration

```bash
enable
configure terminal
hostname R0

interface fa2/0
ip address 192.168.20.1 255.255.255.192
no shutdown
exit

interface se0/0
ip address 192.168.20.193 255.255.255.252
clock rate 64000
no shutdown
exit

router rip
version 2
no auto-summary
network 192.168.20.0
exit

write memory
exit
```

### R1 Configuration

```bash
enable
configure terminal
hostname R1

interface fa2/0
ip address 192.168.20.65 255.255.255.192
no shutdown
exit

interface se1/0
ip address 192.168.20.194 255.255.255.252
no shutdown
exit

interface se0/0
ip address 192.168.20.197 255.255.255.252
clock rate 64000
no shutdown
exit

router rip
version 2
no auto-summary
network 192.168.20.0
exit

write memory
exit
```

### R2 Configuration

```bash
enable
configure terminal
hostname R2

interface fa2/0
ip address 192.168.20.129 255.255.255.192
no shutdown
exit

interface se1/0
ip address 192.168.20.198 255.255.255.252
no shutdown
exit

router rip
version 2
no auto-summary
network 192.168.20.0
exit

write memory
exit
```

---

## 4. Verification

The RIP configuration was verified on all three routers using:

```bash
show ip protocols
```

The expected verification is:

```text
Routing Protocol is "rip"
Default version control: send version 2, receive 2
...
Automatic network summarization is not in effect
Routing for Networks:
    192.168.20.0
```

### R0 – Actual Verification

![R0 RIPv2 Verification](./screenshots/r0-ripv2-verification.png)

### R1 – Actual Verification

![R1 RIPv2 Verification](./screenshots/r1-ripv2-verification.png)

### R2 – Actual Verification

![R2 RIPv2 Verification](./screenshots/r2-ripv2-verification.png)

The screenshots above are the actual Packet Tracer execution/verification from the practical.

---

## 5. Connectivity Testing

After configuring RIPv2, connectivity can be tested from the PCs using `ping`.

### From PC0

```bash
ping 192.168.20.70
ping 192.168.20.140
```

### From PC3

```bash
ping 192.168.20.140
```

Successful replies confirm that the routers are learning and forwarding routes between the different LANs.

---

## 6. Important RIPv2 Commands

| Command | Purpose |
|---|---|
| `router rip` | Enters RIP configuration mode |
| `version 2` | Enables RIPv2 |
| `no auto-summary` | Prevents automatic classful route summarization |
| `network 192.168.20.0` | Enables RIP on interfaces belonging to the major network |
| `show ip protocols` | Verifies RIP configuration |
| `show ip route` | Displays the routing table |
| `show ip route rip` | Displays routes learned through RIP |
| `write memory` | Saves the router configuration |

---

## 7. RIPv1 vs RIPv2

| Feature | RIPv1 | RIPv2 |
|---|---|---|
| Routing type | Classful | Classless |
| Subnet mask in updates | No | Yes |
| VLSM support | No | Yes |
| Update delivery | Broadcast | Multicast `224.0.0.9` |
| Authentication | Not supported | Supported |
| Auto-summary | Always on | Configurable |

RIPv2 is required for this practical because the addressing scheme uses **different subnet masks within the same major network** (`/26` and `/30`).

---

## 8. Result

The three-router Cisco Packet Tracer network was configured successfully using **RIPv2 with VLSM**. RIPv2 was enabled on R0, R1, and R2, automatic summarization was disabled, and the configuration was verified using `show ip protocols`.

The practical demonstrates how RIPv2 can advertise classless routes while supporting a VLSM-based network design.

---

## 9. Skills Demonstrated

- Cisco Packet Tracer network design
- Router and switch configuration
- IPv4 addressing
- VLSM subnetting
- RIPv2 configuration
- Dynamic routing
- Serial and FastEthernet configuration
- Routing verification using Cisco IOS commands
- Network connectivity testing using `ping`

> **Note:** The screenshots in this repository are the actual execution screenshots from the practical.
