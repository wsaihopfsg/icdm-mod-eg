# ICDM-RX/MOD Modbus Routing Logic Document

This document defines the Modbus network routing topologies, firmware features, and advanced device configurations for ICDM-RX/MOD gateways, serving as a structured routing logic reference for the AI configurator tool.

---

## 1. Firmware Definitions

ICDM-RX/MOD gateways support two primary firmware applications. All gateways are loaded with **Modbus Router** firmware at the factory.

### 1.1 Modbus Router Firmware
**Modbus Router** is the standard, high-performance Modbus gateway firmware designed for Modbus-centric networks, security isolation, and complex address conflict resolution.
*   **Default State:** Factory-loaded on all ICDM-RX/MOD chassis.
*   **Exclusive Features:**
    *   **Shared Memory Sub-system:** Provides master-to-master communication using a configurable memory interface consisting of eight 200 Holding Register blocks and eight 160 Coil blocks.
    *   **Private Modbus Serial Bus:** Isolates serial master and slave communication on a single port from the public network while allowing public network integration.
    *   **Disable Writes (Read-Only) Protection:** Restricts write commands on a per-port basis, blocking unauthorized data modification.
    *   **Device ID Offset:** Adds or subtracts a configurable offset to the Modbus Device ID right before transmitting messages out of a serial port.
    *   **Remote Modbus/TCP Device Routing:** Provides native network-wide connectivity to remote Modbus/TCP slaves or serial Modbus slaves connected to other gateways.

### 1.2 Modbus/TCP Firmware
**Modbus/TCP** firmware is a specialized application designed for integrating serial Raw/ASCII devices into Ethernet Modbus networks and for peer-to-peer queue-based controller communication.
*   **Activation:** Must be manually uploaded to the gateway.
*   **Core Characteristics:**
    *   **Queued Messages Sub-system:** Connects serial ports and internal TCP/IP sockets to form an asynchronous, queued holding register interface, enabling both master-to-master and slave-to-slave communication.
    *   **Multi-Client Raw/ASCII Access:** Allows both Modbus controllers and applications to communicate to a single raw/ASCII device simultaneously.
    *   **Shared Feature:** Supports the **Alias Device ID** functionality (developed primarily to help resolve device ID conflicts for Modbus masters by modifying IDs upon packet receipt).

---

## 2. Base Topologies (Section 2 Reference)

This section maps all controller-to-device configurations defined in Section 2 of the manual. 

> **Remote Serial Tunneling Rule:** Any topology categorized as "Remote" requires a minimum of **two gateways** (one local gateway at the serial controller end and one remote gateway at the device end) to tunnel serial communications across an Ethernet TCP/IP network.

### 2.1 Modbus/TCP Master Controller
Controllers include PLCs, OPC Servers, SCADA Systems, and HMIs communicating as Modbus/TCP masters.

*   **Modbus/TCP Master -> Serial Modbus/RTU Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.1.1
*   **Modbus/TCP Master -> Serial Modbus/ASCII Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.1.2
*   **Modbus/TCP Master -> Serial Raw/ASCII Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.1.3
*   **Modbus/TCP Master -> Ethernet Raw/ASCII Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.1.4

### 2.2 Modbus/TCP Slave Controller
Controllers communicating only as slaves via Modbus/TCP. *Functionality is strictly limited to communicating with raw/ASCII devices.*

*   **Modbus/TCP Slave -> Raw/ASCII Serial Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.2.1
*   **Modbus/TCP Slave -> Raw/ASCII Ethernet Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.2.2

### 2.3 Modbus/TCP Master/Slave Controller
Full-featured controllers communicating simultaneously as both master and slave via Modbus/TCP. *Functionality is strictly limited to communicating with raw/ASCII devices.*

*   **Modbus/TCP Master/Slave -> Raw/ASCII Serial Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.3.1
*   **Modbus/TCP Master/Slave -> Raw/ASCII Ethernet Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.3.2

### 2.4 Modbus/RTU or Modbus/ASCII Serial Master Controller
Controllers communicating as Modbus masters via a local serial COM port.

*   **Serial Modbus Master -> Serial Modbus/RTU Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.4.1
*   **Serial Modbus Master -> Serial Modbus/ASCII Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.4.2
*   **Serial Modbus Master -> Modbus/TCP Slaves**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.4.3
*   **Serial Modbus Master -> Serial Raw/ASCII Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.4.4
*   **Serial Modbus Master -> Raw/ASCII Ethernet Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.4.5
*   **Serial Modbus Master -> Remote Serial Modbus/RTU Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.4.6
    *   **Tunneling Requirement:** **Multiple Gateways (2 or more)**
*   **Serial Modbus Master -> Remote Modbus/ASCII Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.4.7
    *   **Tunneling Requirement:** **Multiple Gateways (2 or more)**
*   **Serial Modbus Master -> Remote Raw/ASCII Serial Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** **Modbus Router**
    *   **Section Number:** 2.4.8
    *   **Tunneling Requirement:** **Multiple Gateways (2 or more)**

### 2.5 Modbus/RTU or Modbus/ASCII Serial Slave Controller
Controllers communicating as Modbus slaves via a local serial COM port. *Functionality is strictly limited to communicating with raw/ASCII devices.*

*   **Serial Modbus Slave -> Raw/ASCII Serial Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.5.1
*   **Serial Modbus Slave -> Raw/ASCII Ethernet Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.5.2
*   **Serial Modbus Slave -> Remote Raw/ASCII Serial Device(s)**
    *   **Recommended Firmware:** **Modbus Router** or **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.5.3
    *   **Tunneling Requirement:** **Multiple Gateways (2 or more)**

### 2.6 Modbus/RTU or Modbus/ASCII over Ethernet TCP/IP Master
Masters (e.g., OPC Servers, SCADA Systems) that communicate as a master via an Ethernet TCP/IP connection or COM port redirector.

*   **Modbus over Ethernet Master -> Modbus/RTU Serial Slave(s)**
    *   **Recommended Firmware:** **Modbus Router** (Optionally *Modbus Server* firmware on specific DeviceMaster models)
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.6.1
*   **Modbus over Ethernet Master -> Modbus/ASCII Serial Slave(s)**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.6.2
*   **Modbus over Ethernet Master -> Modbus/TCP Slaves**
    *   **Recommended Firmware:** **Modbus Router**
    *   **Alternate Firmware:** **Modbus/TCP**
    *   **Section Number:** 2.6.3

### 2.7 Application communicating via Raw/ASCII over Ethernet TCP/IP
Control or database applications receiving/transmitting Raw/ASCII data over Ethernet or COM port redirector.

*   **Ethernet Raw/ASCII Application -> Raw/ASCII Serial Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.7.1
*   **Ethernet Raw/ASCII Application -> Raw/ASCII Ethernet Device(s)**
    *   **Recommended Firmware:** **Modbus/TCP**
    *   **Alternate Firmware:** None
    *   **Section Number:** 2.7.2

---

## 3. Controller-to-Controller Communication (Section 3 Reference)

When connecting Modbus controllers to other Modbus controllers, the routing architecture varies depending on whether the controllers are masters or slaves.

### 3.1 Shared Memory (Modbus Router Firmware)
*   **Scope:** Used strictly for **Master-to-Master** communication.
*   **System Layout:** Contains eight 200 Holding Register blocks and eight 160 Coil blocks.
*   **Read/Write Rules:**
    *   All Modbus masters (Modbus/TCP, serial Modbus RTU/ASCII, and Modbus over Ethernet TCP/IP) can read Shared Memory.
    *   Write access can be configured per Holding Register and Coil block. Access can be granted to all masters or restricted to a port-specific serial master, a Modbus/TCP master, or an Ethernet TCP/IP master.
*   **Diagnostics:** Web interface displays and clears memory. Blocks maintain diagnostic counts for read, write, and blocked write counts (blocked write attempts are saved to the Write Violation log).
*   **Subnet Tunneling Architectures:**
    *   **Using 1-Port Gateways (Section 3.1.3.1):** Requires **two gateways** (one on each subnet) connected via a physical crossover serial link (RS-232/485/422). The master port of Subnet 1's gateway is wired to the slave port of Subnet 2's gateway, and vice versa.
    *   **Using 2-Port Gateways (Section 3.1.3.2):** Requires **two gateways** connected via physical serial crossovers across the subnets.

### 3.2 Queued Messages (Modbus/TCP Firmware)
*   **Scope:** Enabled for **Master-to-Master** AND **Slave-to-Slave** communication.
*   **System Layout:** Asynchronous, queued holding register interface. Bi-directional data paths are created by logically connecting serial ports and/or internal TCP/IP sockets together.
*   **Architectures:**
    *   **Two Modbus/TCP Masters (Section 3.2.1.1.1):** Direct master-to-master link using either internal TCP/IP socket connections or physical cable loops connecting two serial ports on a multi-port gateway.
    *   **Two Serial Modbus Masters (Section 3.2.1.1.2):** Communication enabled via an internal TCP/IP socket bridge. Multi-serial connections can be made using a 4-Port ICDM-RX/MOD.
    *   **Modbus/TCP Master to Serial Modbus Master (Section 3.2.1.1.3):** Bridged using an internal TCP/IP socket connection on the gateway.
    *   **One-to-Many Masters (Section 3.2.1.2):** One master sends data via a "Write Holding Registers" message, which the gateway queues. All other masters retrieve this data by sending "Read Holding Registers" requests.
    *   **Slave-to-Slave Architectures (Section 3.2.2):** Integrates two Modbus/TCP slaves (Section 3.2.2.1), two Serial Modbus slaves (Section 3.2.2.1.1), or a Modbus/TCP slave and a Serial Modbus slave (Section 3.2.2.2) via internal TCP/IP socket connections on the gateway.

---

## 4. Advanced Modifiers (Sections 4, 5, & 6 Reference)

These configurations apply modifications to the standard routing pathways to achieve security, write protection, or address conflict resolution.

### 4.1 Private Modbus Serial Bus (Section 4)
A Private Modbus Serial Bus isolates a local serial network from the broader public Modbus network while providing security and fault tolerance.

*   **Hardware Requirements:** Must include a **Serial Modbus Master** and one or more **Serial Modbus Slaves** connected on the *same physical serial bus* attached to a port on the ICDM-RX/MOD.
*   **Required Firmware:** **Modbus Router**
*   **Routing & Access Logic:**
    1.  Only the local Serial Master can directly communicate with the local private slaves.
    2.  Public Modbus masters on the network are strictly blocked from sending messages to the private slaves.
    3.  The local Serial Master can access the public network to communicate with public slaves, Modbus/TCP slaves, or remote slaves via Remote Device Routing.
    4.  Data exchange between the isolated Serial Master and public masters is handled via the gateway's Shared Memory.
*   **Fault Tolerance Benefit:** If the gateway loses power or its Ethernet link is broken, the Serial Master and private slaves can continue local communications uninterrupted. It also eliminates gateway response latency and congestion.

### 4.2 Read-Only Modbus Protection (Section 5)
Blocks standard Modbus write messages on a per-port basis to prevent unauthorized data modification.

*   **Required Firmware:** **Modbus Router**
*   **Mechanism:** Enabling the **Disable Writes (Read Only)** option blocks all standard write commands. The gateway rejects write messages with an "Illegal Function Code" response and logs them to the Write Violation log.
*   **Topologies:**
    *   **Standard Per-Port (Section 5.1):** Enabled on individual ports via the Serial Configuration page. Enabling it on all ports makes the entire gateway read-only.
    *   **Dual-Port Device Isolation (Section 5.2.2):** Used when a field device has two serial ports. Requires **two gateways**. One port of the device is connected to a gateway configured as Read-Only (connected to the public monitoring network), while the other port is connected to a gateway configured as Read/Write (connected to a private, secure control network).
    *   **Serial + Ethernet Device Isolation (Section 5.2.3):** Used when a field device has one serial port and one Ethernet port. Requires **one gateway**. The device's Ethernet port connects directly to the private secure control network (Read/Write access). The device's serial port connects to an ICDM-RX/MOD gateway with **Disable Writes (Read Only)** enabled, which is then connected to the public network.

### 4.3 Resolving Modbus Device ID Conflicts (Section 6)
Resolves addressing conflicts on networks where multiple devices share the same Modbus Device ID, without requiring modifications to the master program logic or slave hardware.

*   **Required Firmware:** **Modbus Router**
*   **Core Functions:** Utilizes **Alias Device ID** (modifies received Device ID immediately upon packet reception), **Device ID Offset** (adds/subtracts offset right before transmitting out a serial port), and **Remote Device Routing** (routes to remote IPs based on ID).

#### 6.5.1 Modbus/TCP Master to Local Slaves with Same Device ID
*   **Combination:** **1 Modbus/TCP Master** communicating with multiple Modbus RTU/ASCII serial slaves on different serial ports of the *same local gateway*, where the slaves share the same actual Device ID.
*   **Gateways Required:** **1 gateway**
*   **Routing Logic:** 
    *   Devices on Port 1 are accessed with Offset = 0 (actual IDs, e.g., ID 1, 2).
    *   Devices on Port 2 are configured with **Device ID Offset = Subtract 10**.
    *   The master accesses Port 2 devices using virtual IDs (ID 11, 12). When the gateway receives a message for ID 11 or 12, it subtracts 10, transmits ID 1 or 2 to the serial port, receives the response, adds 10 back, and returns the response to the master.

#### 6.5.2 Modbus Serial Master to Local and Remote Devices with Same Device IDs
*   **Combination:** **1 Modbus Serial Master** communicating with local serial slaves and remote serial slaves sharing duplicate Device IDs.
*   **Gateways Required:** **3 gateways**
    *   *Gateway 1 (Local Master side):* **Modbus Router** firmware.
    *   *Gateway 2 (Remote Slave A side):* **Modbus/TCP** firmware (No Alias).
    *   *Gateway 3 (Remote Slave B side):* **Modbus/TCP** firmware (with Alias).
*   **Routing Logic:** 
    *   The master accesses Gateway 2's remote device (actual ID 255) through Gateway 1's Remote Device configuration, which routes ID 255 to Gateway 2's IP.
    *   The master accesses Gateway 3's remote device (actual ID 255) using virtual ID 200. Gateway 1 routes ID 200 to Gateway 3's IP. Gateway 3 is configured with **Alias Device ID (Rx Device ID = 200, Alias Device ID = 255)**, mapping the incoming ID 200 packet to ID 255 before transmitting it to the slave.

#### 6.5.3 Modbus Serial Master to Two Remote Serial Raw/ASCII Devices
*   **Combination:** **1 Modbus Serial Master** communicating with two remote raw/ASCII serial devices.
*   **Gateways Required:** **3 gateways** (1 local gateway at the serial master, 1 remote gateway at each of the two remote raw/ASCII devices).
*   **Routing Logic:** Uses Remote Device Routing on the local master gateway and Alias Device ID configurations on the remote gateways to route serial communications seamlessly across the network.

#### 6.5.4 Merging Two Serial Modbus Networks
*   **Combination:** **2 Modbus Serial Masters** (e.g., PLC and SCADA), each with their own existing local serial network (e.g., AHU/VFD slaves with actual IDs 1, 2). The networks must be merged so each controller has access to all four slaves, without modifying existing slave addresses or master logic.
*   **Gateways Required:** **2 gateways** (one 2-Port gateway placed between each controller and its local slaves).
*   **Routing Logic:**
    *   Both gateways run **Modbus Router** with **Remote Device Routing** and **Alias Device ID** configurations.
    *   Local devices are accessed directly by their local controller using actual IDs (1, 2).
    *   Remote devices are accessed using virtual IDs (e.g., Controller 1 accesses Controller 2's devices as IDs 21 and 22).
    *   Gateway 1 routes IDs 21 and 22 to Gateway 2. When Gateway 2 receives them, its **Alias Configuration** maps Rx Device ID 21 to Alias ID 1, and Rx Device ID 22 to Alias ID 2, before transmitting to the slaves.
    *   Gateway 2 routes IDs 31 and 32 to Gateway 1, which aliases them back to IDs 1 and 2 for local transmission.

#### 6.5.5 Modbus Connectivity between Separate Ethernet Networks
*   **Combination:** Modbus controllers on one Ethernet subnet communicating with remote Modbus slaves on a separate Ethernet subnet through an IP router.
*   **Gateways Required:** **2 or more gateways** (at least one on each Ethernet subnet).
*   **Routing Logic:** Leverages **Remote Device Routing** on the master-side gateway to target the IP address of the slave-side gateway across the router. If address conflicts exist, **Alias Device ID** is applied at the destination gateway.

---
## 5. System Output Examples
Use the following examples to format your JSON output and understand when to trigger specific checkboxes.

**Example 1: Remote Serial Tunneling**
User: "I have a Modbus TCP master connecting to a remote RTU slave."
Output:
{
  "node1": "tcp_master",
  "node2": "rtu_slave",
  "checkboxes": ["remote_devices"],
  "radio": null,
  "explanation": "<strong>AI Recommendation:</strong> Based on Section 2.4.6, routing a Modbus/TCP Master to a remote Modbus RTU slave over Ethernet requires the Modbus Router firmware and 2 gateways for serial tunneling."
}

**Example 2: Standard Master-to-Master (With Alternate Firmware)**
User: "I need to connect a Modbus TCP master to another Modbus TCP master."
Output:
{
  "node1": "tcp_master",
  "node2": "tcp_master",
  "checkboxes": [],
  "radio": null,
  "explanation": "<strong>AI Recommendation:</strong> For communication between two Modbus/TCP Masters, the Modbus Router firmware using the Shared Memory sub-system is recommended (Section 3.1.1.1). Alternatively, you can use the Modbus/TCP firmware to achieve this using Queued Messages (Section 3.2.1.1.1)."
}

**Example 3: One Master to Multiple Masters**
User: "I need one Modbus master to communicate with multiple Modbus masters."
Output:
{
  "node1": "tcp_master",
  "node2": "tcp_master",
  "checkboxes": ["multi_master"],
  "radio": null,
  "explanation": "<strong>AI Recommendation:</strong> Based on Section 3.2.1.2, connecting one Modbus Master to multiple Modbus Masters requires the Modbus/TCP firmware using the Queued Messages sub-system."
}

