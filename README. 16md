# Cisco Packet Tracer Practical – Configuring BGP Routing

## Aim

To configure **BGP (Border Gateway Protocol)** in Cisco Packet Tracer using two Autonomous Systems and verify BGP routing and connectivity between the networks.

## Practical Overview

This practical demonstrates **eBGP (External BGP)** between two different Autonomous Systems:

- **R0 → AS 100**
- **R1 → AS 200**
- **S0 and S1 → LAN switches**
- **PC0–PC3 → End devices**
- A direct **serial link** connects R0 and R1.
- BGP is used to advertise the LAN networks between the two autonomous systems.

BGP is an **Exterior Gateway Protocol (EGP)** used for routing between separate autonomous systems. Unlike RIP, OSPF, and EIGRP, which are Interior Gateway Protocols (IGPs), BGP is designed for inter-AS routing.

---

## 1. Network Topology

The topology contains two routers representing two different organizations/autonomous systems. Each router connects to one LAN through a switch, and R0 and R1 are connected through a serial eBGP link.

![Network Topology](./screenshots/network-topology.png)

### Devices Used

| Device | Quantity |
|---|---:|
| Router-PT-Empty | 2 |
| 2960 Switch | 2 |
| PCs | 4 |

### Autonomous Systems

| Router | Autonomous System |
|---|---:|
| R0 | AS 100 |
| R1 | AS 200 |

---

## 2. Cabling

### LAN Connections

| From | To | Interface |
|---|---|---|
| PC0 | S0 | Fa0/1 |
| PC1 | S0 | Fa0/2 |
| S0 | R0 | Fa0/24 → Fa2/0 |
| PC2 | S1 | Fa0/1 |
| PC3 | S1 | Fa0/2 |
| S1 | R1 | Fa0/24 → Fa2/0 |

### BGP Peering Connection

| From | To | Interface |
|---|---|---|
| R0 | R1 | Se0/0 ↔ Se1/0 |

The serial connection between R0 and R1 is the **eBGP peering link**.

---

## 3. IP Addressing Scheme

| Network | Purpose | Subnet Mask |
|---|---|---|
| `172.16.1.0/24` | AS 100 LAN | `255.255.255.0` |
| `172.16.2.0/24` | AS 200 LAN | `255.255.255.0` |
| `203.0.113.0/30` | R0 ↔ R1 eBGP link | `255.255.255.252` |

### PC IP Configuration

| PC | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC0 | `172.16.1.10` | `255.255.255.0` | `172.16.1.1` |
| PC1 | `172.16.1.11` | `255.255.255.0` | `172.16.1.1` |
| PC2 | `172.16.2.10` | `255.255.255.0` | `172.16.2.1` |
| PC3 | `172.16.2.11` | `255.255.255.0` | `172.16.2.1` |

---

## 4. R0 Configuration – AS 100

```bash
enable
configure terminal
hostname R0

interface fa2/0
ip address 172.16.1.1 255.255.255.0
no shutdown
exit

interface se0/0
ip address 203.0.113.1 255.255.255.252
clock rate 64000
no shutdown
exit

router bgp 100
neighbor 203.0.113.2 remote-as 200
network 172.16.1.0 mask 255.255.255.0
exit

write memory
exit
```

### R0 BGP Configuration – Actual Screenshot

![R0 BGP Configuration](./screenshots/r0-bgp-configuration.png)

The practical screenshot shows the BGP process being configured with:

```text
router bgp 100
network 172.16.1.0 mask 255.255.255.0
```

and the configuration being saved using `write memory`.

---

## 5. R1 Configuration – AS 200

```bash
enable
configure terminal
hostname R1

interface fa2/0
ip address 172.16.2.1 255.255.255.0
no shutdown
exit

interface se1/0
ip address 203.0.113.2 255.255.255.252
no shutdown
exit

router bgp 200
neighbor 203.0.113.1 remote-as 100
network 172.16.2.0 mask 255.255.255.0
exit

write memory
exit
```

### R1 BGP Configuration – Actual Screenshot

![R1 BGP Configuration](./screenshots/r1-bgp-configuration.png)

The screenshot shows the R1 BGP network statement and the configuration being saved successfully.

---

## 6. Understanding the BGP Commands

| Command | Purpose |
|---|---|
| `router bgp 100` | Starts the BGP process for AS 100 |
| `router bgp 200` | Starts the BGP process for AS 200 |
| `neighbor 203.0.113.2 remote-as 200` | Defines R1 as the BGP neighbor of R0 |
| `neighbor 203.0.113.1 remote-as 100` | Defines R0 as the BGP neighbor of R1 |
| `network 172.16.1.0 mask 255.255.255.0` | Advertises the AS 100 LAN |
| `network 172.16.2.0 mask 255.255.255.0` | Advertises the AS 200 LAN |
| `show ip bgp summary` | Checks BGP neighbor status |
| `show ip bgp` | Displays the BGP routing table |
| `show ip route bgp` | Displays routes learned through BGP |
| `write memory` | Saves the configuration |

> **Important:** A BGP `network` statement advertises a prefix only when that prefix already exists in the router's routing table.

---

## 7. BGP Routing Table Verification

The BGP table can be checked with:

```bash
show ip bgp
```

The routing table can also be inspected using:

```bash
show ip route
```

and specifically for BGP-learned routes:

```bash
show ip route bgp
```

### Actual R0 Routing Table Screenshot

![R0 BGP Routing Table](./screenshots/r0-bgp-routing-table.png)

The screenshot shows the BGP table/routing information on R0. The configured local network is present, while the BGP table can be used to verify routes learned through the BGP peering.

---

## 8. Connectivity Testing

Connectivity between the two autonomous systems can be tested from the PCs.

### From PC0

```bash
ping 172.16.2.10
```

### From PC2

```bash
ping 172.16.1.10
```

### Actual Packet Tracer Connectivity Test

![PC Connectivity Test](./screenshots/pc-connectivity-test.png)

The uploaded screenshot shows the Packet Tracer PC command prompts being used to test connectivity toward the remote LAN addresses.

---

## 9. Packet Tracer Network Verification

### Topology Verification

![Topology Verification](./screenshots/network-topology-verification.png)

The topology shows:

- R0 connected to S0
- R1 connected to S1
- Two PCs connected to each switch
- R0 and R1 connected through the serial link

### Packet Flow / Simulation View

![Packet Flow](./screenshots/packet-tracer-packet-flow.png)

The Packet Tracer simulation view shows packets being exchanged through the configured network.

### Final Topology View

![Final Topology](./screenshots/packet-tracer-topology-final.png)

---

## 10. BGP vs IGP

BGP differs from protocols such as RIP, OSPF, and EIGRP because it is designed for communication between autonomous systems.

| Feature | BGP | RIP / OSPF / EIGRP |
|---|---|---|
| Protocol type | EGP | IGP |
| Main purpose | Inter-AS routing | Intra-AS routing |
| Autonomous Systems | Multiple ASes | Generally within one AS |
| BGP neighbor | Explicitly configured | Protocol-specific discovery/adjacency |
| Route advertisement | `network` / policy mechanisms | Protocol-specific |

---

## 11. eBGP vs iBGP

### eBGP

**External BGP** is used between routers belonging to different autonomous systems.

In this practical:

```text
AS 100  ←── eBGP ──→  AS 200
 R0                    R1
```

### iBGP

**Internal BGP** is used between routers inside the same autonomous system. It is outside the scope of this practical.

---

## 12. Practical Result

The two-router, two-AS topology was configured in Cisco Packet Tracer with:

- **R0 in AS 100**
- **R1 in AS 200**
- An **eBGP peering link** between R0 and R1
- LAN `172.16.1.0/24` advertised from AS 100
- LAN `172.16.2.0/24` advertised from AS 200

BGP configuration, routing information, and Packet Tracer network operation were verified using Cisco IOS commands and the uploaded practical screenshots.

---

## 13. Skills Demonstrated

- Cisco Packet Tracer
- BGP configuration
- eBGP peering
- Autonomous Systems
- IPv4 addressing
- Serial interface configuration
- FastEthernet configuration
- Dynamic routing
- BGP route advertisement
- Routing-table verification
- Network connectivity testing
- Cisco IOS CLI

---

## Screenshots

All screenshots in this README are from the actual Packet Tracer practical execution.

### 1. PC Connectivity Test
![Connectivity Test](./screenshots/pc-connectivity-test.png)

### 2. Network Topology
![Network Topology](./screenshots/network-topology.png)

### 3. R0 BGP Routing Table
![R0 BGP Routing Table](./screenshots/r0-bgp-routing-table.png)

### 4. R0 BGP Configuration
![R0 BGP Configuration](./screenshots/r0-bgp-configuration.png)

### 5. R1 BGP Configuration
![R1 BGP Configuration](./screenshots/r1-bgp-configuration.png)
