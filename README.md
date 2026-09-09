# 🌐 VLAN Trunking Protocol Configuration Using Cisco Packet Tracer

![Cisco](https://img.shields.io/badge/Cisco-Catalyst%202960-blue)
![Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-Lab-green)
![VLAN](https://img.shields.io/badge/Networking-VLAN-orange)
![Switching](https://img.shields.io/badge/Technology-Switching-red)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

A hands-on **Cisco Packet Tracer** project demonstrating how to configure **VLANs, access ports, trunk ports, and basic VLAN connectivity** using Cisco Catalyst 2960 switches.

This lab introduces fundamental **Layer 2 switching concepts** and demonstrates how VLANs logically separate devices into different broadcast domains.

---

## 📌 Overview

In this project, two Cisco Catalyst 2960 switches are configured with three VLANs:

* **VLAN 10 — IT**
* **VLAN 20 — HR**
* **VLAN 30 — Finance**

The VLANs are configured on both switches, and the switches are connected using a **trunk link**.

The trunk allows traffic belonging to multiple VLANs to travel between the switches.

> **Key concept:** Devices in the same VLAN can communicate across switches when the trunk is correctly configured, while devices in different VLANs cannot communicate by default.

Inter-VLAN communication requires a Layer 3 device and is outside the scope of this basic VLAN lab.

---

## 🎯 Project Objectives

The main objectives of this project are to:

* Understand VLAN concepts
* Create VLANs on Cisco switches
* Assign names to VLANs
* Configure access ports
* Assign switch ports to VLANs
* Configure trunk ports
* Configure IPv4 addresses on hosts
* Verify VLAN membership
* Test connectivity between devices
* Understand broadcast-domain separation
* Understand the purpose of Inter-VLAN Routing

---

## 🖥️ Network Topology

The topology consists of:

* 2 × Cisco Catalyst 2960 switches
* 6 × PCs
* Ethernet connections between PCs and switches
* 1 × switch-to-switch trunk connection

```text
                         TRUNK LINK
                    ┌─────────────────┐
                    │                 │
             ┌──────┴──────┐   ┌──────┴──────┐
             │   Switch 0  │   │   Switch 1  │
             │  Catalyst    │   │  Catalyst   │
             │     2960     │   │     2960    │
             └──────────────┘   └─────────────┘
                │   │   │          │   │   │
                │   │   │          │   │   │
              VLAN VLAN VLAN      VLAN VLAN VLAN
               10   20   30        10   20   30
```

---

## 🔌 Devices Used

| Device            | Model / Type            | Quantity | Purpose                          |
| ----------------- | ----------------------- | -------: | -------------------------------- |
| 🖧 Switch         | Cisco Catalyst 2960     |        2 | VLAN configuration and switching |
| 💻 PC             | PC-PT                   |        6 | End devices                      |
| 🔌 Ethernet Cable | Copper Straight-Through |        6 | PC-to-switch connections         |
| 🔗 Ethernet Link  | Switch-to-Switch        |        1 | Trunk connection                 |

---

## 🏗️ VLAN Design

Three VLANs are configured on **both switches**.

| VLAN ID | Department | Network           |
| ------: | ---------- | ----------------- |
|      10 | IT         | `192.168.10.0/24` |
|      20 | HR         | `192.168.20.0/24` |
|      30 | Finance    | `192.168.30.0/24` |

### VLAN Structure

```text
VLAN 10
└── IT
    └── 192.168.10.0/24

VLAN 20
└── HR
    └── 192.168.20.0/24

VLAN 30
└── Finance
    └── 192.168.30.0/24
```

The same VLANs exist on both switches:

```text
Switch 0                         Switch 1

VLAN 10 ───────── TRUNK ─────── VLAN 10
VLAN 20 ───────── TRUNK ─────── VLAN 20
VLAN 30 ───────── TRUNK ─────── VLAN 30
```

---

# ⚙️ Configuration

## 1. Create the Network Topology

Create the following topology in Cisco Packet Tracer:

* Two Cisco Catalyst 2960 switches
* Six PCs
* Three VLAN groups
* One switch-to-switch connection

Connect the PCs to the appropriate switch ports according to the desired VLAN assignments.

---

## 2. Configure Host IP Addresses

Each VLAN uses a separate IPv4 network.

### VLAN 10 — IT

**Network:** `192.168.10.0/24`

| Device | IP Address     | Subnet Mask     |
| ------ | -------------- | --------------- |
| PC0    | `192.168.10.1` | `255.255.255.0` |
| PC1    | `192.168.10.2` | `255.255.255.0` |
| PC2    | `192.168.10.3` | `255.255.255.0` |
| PC3    | `192.168.10.4` | `255.255.255.0` |

### VLAN 20 — HR

**Network:** `192.168.20.0/24`

| Device | IP Address     | Subnet Mask     |
| ------ | -------------- | --------------- |
| PC4    | `192.168.20.1` | `255.255.255.0` |
| PC5    | `192.168.20.2` | `255.255.255.0` |

### VLAN 30 — Finance

**Network:** `192.168.30.0/24`

Example addresses:

```text
192.168.30.1
192.168.30.2
192.168.30.3
192.168.30.4
```

> **Note:** A default gateway is not required for the initial same-VLAN connectivity test because Inter-VLAN Routing has not been configured.

---

# 3. Create VLANs

The VLANs must be created on **both switches**.

Enter privileged EXEC mode:

```bash
Switch> enable
```

Enter global configuration mode:

```bash
Switch# configure terminal
```

### Create VLAN 10 — IT

```bash
Switch(config)# vlan 10
Switch(config-vlan)# name IT
Switch(config-vlan)# exit
```

### Create VLAN 20 — HR

```bash
Switch(config)# vlan 20
Switch(config-vlan)# name HR
Switch(config-vlan)# exit
```

### Create VLAN 30 — Finance

```bash
Switch(config)# vlan 30
Switch(config-vlan)# name Finance
Switch(config-vlan)# exit
```

Repeat the VLAN configuration on the second switch.

---

# 4. Verify VLANs

Use:

```bash
Switch# show vlan brief
```

Expected output will contain entries similar to:

```text
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active
10   IT                               active
20   HR                               active
30   Finance                          active
```

At this stage, the VLANs exist but access ports have not yet been assigned.

---

# 5. Identify Switch Ports

Before assigning ports to VLANs, identify which switch interfaces are connected to each PC.

For example:

```text
PC0 → Fa0/1
PC1 → Fa0/2
PC2 → Fa0/9
PC3 → Fa0/10
```

> **Important:** The actual interface numbers depend on your Packet Tracer topology.

---

# 6. Assign Ports to VLAN 10

For example, if `Fa0/1` and `Fa0/2` are connected to IT PCs:

```bash
Switch(config)# interface range fa0/1 - 2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
```

This configures the interfaces as access ports and assigns them to VLAN 10.

---

# 7. Assign Ports to VLAN 20

For example:

```bash
Switch(config)# interface range fa0/9 - 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit
```

---

# 8. Assign Ports to VLAN 30

For example:

```bash
Switch(config)# interface range fa0/17 - 18
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit
```

Repeat the appropriate configuration on the second switch.

---

# 9. Verify VLAN Port Assignments

Run:

```bash
Switch# show vlan brief
```

Example:

```text
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
10   IT                               active    Fa0/1, Fa0/2
20   HR                               active    Fa0/9, Fa0/10
30   Finance                          active    Fa0/17, Fa0/18
```

This confirms that the access ports have been assigned to the correct VLANs.

---

# 🔗 10. Configure the Trunk Link

The connection between the two switches must operate as a **trunk**.

A trunk allows multiple VLANs to use a single physical link.

```text
Switch 0 ================= Switch 1
             TRUNK
          VLAN 10,20,30
```

### Switch 0

Assuming the switch-to-switch connection uses `Fa0/24`:

```bash
Switch(config)# interface fa0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

### Switch 1

Configure the corresponding interface:

```bash
Switch(config)# interface fa0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

> **Note:** Use the actual interfaces connected between your switches. The interface number may differ depending on your topology.

---

# 🔍 11. Verify the Trunk

Use:

```bash
Switch# show interfaces trunk
```

The output should show the interface operating as a trunk and indicate the VLANs that are active/allowed.

---

# 💾 12. Save the Configuration

Save the configuration on both switches:

```bash
Switch# copy running-config startup-config
```

Alternatively:

```bash
Switch# write
```

---

# 🧪 Testing & Verification

## Same-VLAN Communication

Devices belonging to the same VLAN should communicate even when they are connected to different switches, provided that:

* The VLAN exists on both switches
* The access ports are correctly assigned
* The trunk is operational
* The hosts have compatible IP addressing

Example:

```text
PC0
192.168.10.1
   │
   │ VLAN 10
   ▼
Switch 0
   │
   │ Trunk
   ▼
Switch 1
   │
   │ VLAN 10
   ▼
PC3
192.168.10.4
```

From PC0:

```bash
ping 192.168.10.4
```

Expected result:

```text
Reply from 192.168.10.4
```

---

## 🚫 Different-VLAN Communication

Devices in different VLANs should **not communicate by default** because each VLAN is a separate Layer 2 broadcast domain.

For example:

```bash
PC0> ping 192.168.20.1
```

Expected result:

```text
Request timed out.
```

This is expected behavior in a Layer 2-only VLAN configuration.

---

# 🌐 Inter-VLAN Routing

Inter-VLAN communication is intentionally not configured in this project.

Therefore:

```text
VLAN 10 ❌ VLAN 20
VLAN 10 ❌ VLAN 30
VLAN 20 ❌ VLAN 30
```

To allow communication between different VLANs, a Layer 3 routing mechanism is required.

Common solutions include:

* **Router-on-a-Stick**
* **Layer 3 Switch**
* **Inter-VLAN Routing**

These concepts can be explored in a separate lab.

---

# 📊 Configuration Summary

| Configuration                        | Purpose                                    |
| ------------------------------------ | ------------------------------------------ |
| VLAN 10                              | Creates the IT broadcast domain            |
| VLAN 20                              | Creates the HR broadcast domain            |
| VLAN 30                              | Creates the Finance broadcast domain       |
| VLAN Name                            | Identifies the purpose of a VLAN           |
| Access Port                          | Connects an endpoint to a VLAN             |
| `switchport access vlan`             | Assigns an access port to a VLAN           |
| Trunk Port                           | Carries traffic for multiple VLANs         |
| `switchport mode trunk`              | Configures a switch interface as a trunk   |
| Host IP Address                      | Provides Layer 3 addressing                |
| `show vlan brief`                    | Verifies VLANs and access-port assignments |
| `show interfaces trunk`              | Verifies trunk configuration               |
| `ping`                               | Tests IP connectivity                      |
| `copy running-config startup-config` | Saves the configuration                    |

---

# 🧠 Key Networking Concepts

## What is a VLAN?

A **Virtual Local Area Network (VLAN)** logically separates a physical switched network into multiple Layer 2 networks.

For example:

```text
                  Switch
                    │
        ┌───────────┼───────────┐
        │           │           │
     VLAN 10     VLAN 20     VLAN 30
        │           │           │
        IT          HR       Finance
```

Each VLAN represents a separate **broadcast domain**.

---

## Access Port vs Trunk Port

| Feature            | Access Port              | Trunk Port              |
| ------------------ | ------------------------ | ----------------------- |
| Typical connection | PC / End Device          | Switch-to-Switch        |
| VLANs carried      | One VLAN                 | Multiple VLANs          |
| Configuration      | `switchport mode access` | `switchport mode trunk` |
| Main purpose       | Connect endpoint         | Carry multiple VLANs    |

---

# 🧪 Verification Checklist

* [x] Network topology created
* [x] PCs assigned IP addresses
* [x] VLAN 10 created
* [x] VLAN 20 created
* [x] VLAN 30 created
* [x] VLANs named
* [x] Access ports identified
* [x] Ports assigned to VLANs
* [x] Trunk interfaces identified
* [x] Trunk configured between switches
* [x] VLAN configuration verified
* [x] Same-VLAN communication tested
* [x] Different-VLAN communication tested
* [x] Configuration saved

---

# 🧰 Technologies & Tools

* 🟢 **Cisco Packet Tracer**
* 🖧 **Cisco Catalyst 2960**
* 💻 **Cisco IOS CLI**
* 🌐 **IPv4**
* 🔀 **VLAN**
* 🔗 **802.1Q Trunking**
* 📡 **Ethernet Switching**
* 🧩 **Broadcast Domains**
* 🧪 **ICMP / Ping**

---

# 🧠 Skills Learned

This project provided practical experience with:

* VLAN creation
* VLAN naming
* VLAN segmentation
* Access-port configuration
* Interface-range configuration
* Trunk-port configuration
* Switch-to-switch connectivity
* IPv4 host addressing
* VLAN verification
* Connectivity testing
* Broadcast-domain concepts
* Same-VLAN communication
* Inter-VLAN communication concepts
* Basic Cisco switching troubleshooting

---

# 📂 Repository Structure

```text
Cisco-VLAN-Configuration/
│
├── README.md
│
├── screenshots/
│   ├── topology.png
│   ├── vlan-configuration.png
│   ├── vlan-brief.png
│   ├── trunk-configuration.png
│   └── connectivity-test.png
│
└── Cisco-VLAN-Configuration.pkt
```

---

# 📥 How to Use This Project

## 1. Clone the Repository

```bash
git clone <REPOSITORY-URL>
```

## 2. Open Cisco Packet Tracer

Launch **Cisco Packet Tracer** on your computer.

## 3. Open the Project

Open:

```text
Cisco-VLAN-Configuration.pkt
```

## 4. Explore the Topology

Review the:

* Switches
* PCs
* VLAN assignments
* IP addressing
* Access ports
* Trunk connection

## 5. Access the Switch CLI

Open a switch and select:

```text
CLI
```

## 6. Verify the Configuration

Run:

```bash
show vlan brief
show interfaces trunk
```

## 7. Test Connectivity

From a PC Command Prompt:

```bash
ping <DESTINATION-IP>
```

---

# 📄 Project Files

### 📦 Cisco Packet Tracer File

The `.pkt` file contains the complete VLAN topology, including:

* Cisco Catalyst 2960 switches
* PCs
* VLAN configuration
* Access-port assignments
* Trunk connection
* Host IP addressing

### 📷 Screenshots

The `screenshots/` directory can contain:

* Network topology
* VLAN configuration
* VLAN assignments
* Trunk configuration
* Connectivity testing

---

# 🏁 Conclusion

This project provided hands-on experience with **VLAN configuration and Layer 2 switching using Cisco Packet Tracer**.

The lab demonstrated how to:

1. Create VLANs
2. Assign VLAN names
3. Assign switch ports to VLANs
4. Configure access ports
5. Configure trunk links
6. Verify VLAN configuration
7. Test connectivity between hosts

### Key Result

> **Devices in the same VLAN can communicate across switches through a properly configured trunk, while devices in different VLANs cannot communicate by default.**

To enable communication between different VLANs, **Inter-VLAN Routing** must be configured.

---

# ⭐ Project Status

🟢 **Completed**

| Category       | Details                       |
| -------------- | ----------------------------- |
| **Platform**   | Cisco Packet Tracer           |
| **Devices**    | Cisco Catalyst 2960           |
| **Level**      | Networking Fundamentals       |
| **Technology** | VLAN & Switching              |
| **VLANs**      | VLAN 10, VLAN 20, VLAN 30     |
| **Focus**      | VLAN Configuration & Trunking |
| **Status**     | Completed                     |

---

# 👨‍💻 Author

**Your Name**

If you found this project useful, consider giving the repository a ⭐.
