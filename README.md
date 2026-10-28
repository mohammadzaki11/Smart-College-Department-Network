 # Smart College Department Network

A secure, segmented, and scalable college campus network designed and simulated using Cisco Packet Tracer.

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Tools and Technologies](#tools-and-technologies)
- [Network Architecture](#network-architecture)
- [VLAN and IP Addressing](#vlan-and-ip-addressing)
- [Implementation](#implementation)
- [Network Security](#network-security)
- [Testing and Results](#testing-and-results)
- [Project Outcomes](#project-outcomes)
- [Future Enhancements](#future-enhancements)
- [Conclusion](#conclusion)
- [Project Team](#project-team)
- [References](#references)

## 1. Project Overview

The **Smart College Department Network** is a Computer Networks mini-project that simulates the network infrastructure of a college campus using Cisco Packet Tracer.

The project connects different college departments through a structured network architecture. It uses Virtual Local Area Networks (VLANs) to logically separate departmental traffic and a Cisco router to enable communication between different VLANs.

The network includes dedicated VLANs for Administration, Computer Science and Engineering (CSE), Faculty, Library, and Servers. Trunk links carry traffic for multiple VLANs between network devices, while switch port security helps prevent unauthorized devices from communicating through secured switch ports.

The project demonstrates the practical implementation of network segmentation, inter-VLAN routing, DHCP relay configuration, connectivity verification, and access-layer security.

## 2. Objectives

- Design and simulate a college departmental network using Cisco Packet Tracer.
- Segment the network into multiple VLANs for better organization and isolation.
- Configure inter-VLAN routing using router-on-a-stick.
- Configure trunk links to carry multiple VLANs between switches and the router.
- Configure DHCP relay to forward client DHCP requests to a server on another subnet.
- Implement switch port security using sticky MAC address learning.
- Demonstrate the handling of unauthorized devices through port-security violation controls.
- Verify network connectivity and security configuration through practical testing.

## 3. Tools and Technologies

| Tool / Technology    | Purpose                                               |
| -------------------- | ----------------------------------------------------- |
| Cisco Packet Tracer  | Network design, configuration, and simulation         |
| Cisco 2911 Router    | Inter-VLAN routing                                    |
| Cisco 2960 Switches  | Layer 2 switching and VLAN management                 |
| VLANs                | Logical departmental network segmentation             |
| IEEE 802.1Q          | VLAN identification on trunk links                    |
| Router-on-a-stick    | Routing between VLANs using router subinterfaces      |
| DHCP Relay           | Forwarding DHCP requests across subnets               |
| Sticky MAC Learning  | Learning and retaining MAC addresses on secured ports |
| Switch Port Security | Restricting unauthorized MAC addresses                |
| ICMP Ping            | Testing network connectivity                          |

## 4. Network Architecture

The network uses a core-and-access switching arrangement. Core switches provide connectivity between the network segments, while departmental switches connect end devices to their respective VLANs.

A Cisco 2911 router provides routing between VLANs through subinterfaces configured on a trunk connection. A server is connected to the server network to support the configured network services.

### Network Devices

| Device              | Function                                         |
| ------------------- | ------------------------------------------------ |
| S1 – Core           | Core network switching                           |
| S2 – Core           | Core network connectivity                        |
| S3 – Admin          | Administrative department connectivity           |
| S4 – CSE            | Computer Science department connectivity         |
| S5 – Faculty        | Faculty network connectivity                     |
| S6 – Library        | Library connectivity and server access           |
| Cisco 2911 Router   | Inter-VLAN routing and DHCP relay configuration  |
| Cisco 2960 Switches | VLAN switching and port security                 |
| Server0             | Server-network services                          |
| PCs                 | End-user devices connected to departmental VLANs |

### Network Topology

The topology represents the logical arrangement of routers, core switches, departmental switches, server infrastructure, and end devices.
<img width="1920" height="1080" alt="Screenshot (259)" src="https://github.com/user-attachments/assets/6ae19631-5a51-4257-a932-b44ea8eeabfc" />


&#x20;

## 5. VLAN and IP Addressing

The network is divided into five VLANs. Each VLAN has a separate IP subnet and a corresponding router subinterface that acts as its default gateway.

| VLAN ID | VLAN Name | Network Address | Subnet Mask   | Default Gateway |
| ------- | --------- | --------------- | ------------- | --------------- |
| 10      | ADMIN     | 192.168.10.0/24 | 255.255.255.0 | 192.168.10.1    |
| 20      | CSE       | 192.168.20.0/24 | 255.255.255.0 | 192.168.20.1    |
| 30      | FACULTY   | 192.168.30.0/24 | 255.255.255.0 | 192.168.30.1    |
| 40      | LIBRARY   | 192.168.40.0/24 | 255.255.255.0 | 192.168.40.1    |
| 50      | SERVERS   | 192.168.50.0/24 | 255.255.255.0 | 192.168.50.1    |

The router subinterfaces are configured with 802.1Q encapsulation for their respective VLAN IDs. The server address used as the DHCP relay destination is `192.168.50.10`.

This arrangement provides a dedicated addressing scheme for each department and makes network management easier.

## 6. Implementation

### 6.1 VLAN Configuration

VLANs were configured on the switches to logically separate departmental traffic.

- VLAN 10 — Administration
- VLAN 20 — CSE
- VLAN 30 — Faculty
- VLAN 40 — Library
- VLAN 50 — Servers

End-device access ports are assigned to their appropriate VLANs. This segmentation separates broadcast domains and provides a foundation for applying department-specific network policies.

### 6.2 Trunk Configuration

Trunk links were configured between relevant network devices to carry traffic belonging to multiple VLANs over the same physical link.

IEEE 802.1Q tagging identifies the VLAN associated with each frame on a trunk. The required VLANs must be active and permitted on the relevant trunk links for end-to-end communication.

### 6.3 Inter-VLAN Routing

Router-on-a-stick was implemented using subinterfaces on the Cisco 2911 router.

Each subinterface is assigned an IP address corresponding to the default gateway of a VLAN. The router receives tagged traffic over the trunk link, routes packets between subnets, and forwards them to the appropriate destination VLAN.

Configured gateway addresses:

- `192.168.10.1` — Administration
- `192.168.20.1` — CSE
- `192.168.30.1` — Faculty
- `192.168.40.1` — Library
- `192.168.50.1` — Servers

Inter-VLAN communication depends on correct addressing, routing configuration, and any applicable security policies.

### 6.4 DHCP Relay Configuration

The router subinterfaces use the `ip helper-address` command to forward eligible DHCP client broadcasts to the DHCP server at `192.168.50.10`.

Example configuration:

```text
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.50.10
```

The same relay configuration is applied to the other client VLAN subinterfaces as required.

DHCP address allocation depends on the server being reachable and correctly configured with the appropriate DHCP scopes, gateways, and address ranges.

## 7. Network Security

### 7.1 Switch Port Security

Port security was implemented on selected switch access ports to limit the MAC addresses permitted to communicate through a port.

The configuration uses sticky MAC address learning, a maximum of one secure MAC address, and the `restrict` violation mode.

Example configuration:

```text
interface FastEthernet0/5
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
```

### 7.2 Unauthorized PC Demonstration

The project includes a practical test of the port-security mechanism.

**Working procedure:**

1. Connect the authorized PC to the secured switch port.
2. Allow the switch to learn the authorized PC's MAC address through sticky learning.
3. Attempt to connect a different PC to the same secured port.
4. Observe the switch's response when the new MAC address exceeds the configured secure MAC address limit.
5. Verify the port-security violation counter and the secured MAC address table.

When a violating MAC address attempts to send traffic, `restrict` mode drops the violating traffic and increments the violation counter. The port remains operational, and legitimate traffic from the authorized secure MAC address can continue.

This provides a basic access-layer security mechanism against unauthorized device connections. It is not a replacement for centralized user authentication or a complete network access-control system.

### 7.3 Security Verification Commands

The following Cisco IOS commands can be used to verify the port-security configuration and status:

```text
show port-security
show port-security interface fa0/5
show port-security address
```

These commands display port-security status, secure MAC addresses, violation information, and related interface details.

## 8. Testing and Results

The network was configured and tested in Cisco Packet Tracer to verify connectivity, routing, and port-security behavior.

| Test Case                    | Verification Method                                                                | Result   |
| ---------------------------- | ---------------------------------------------------------------------------------- | -------- |
| VLAN configuration           | Check VLAN assignments and VLAN database                                           | Verified |
| Trunk connectivity           | Inspect trunk status and permitted VLANs                                           | Verified |
| Router interfaces            | Check router subinterface status                                                   | Verified |
| Default gateway connectivity | Ping the configured VLAN gateway                                                   | Passed   |
| Inter-VLAN routing           | Test communication between appropriately configured VLAN endpoints                 | Verified |
| DHCP relay                   | Test client address assignment through the configured relay                        | Verified |
| Port security                | Inspect secured MAC addresses and port-security status                             | Verified |
| Unauthorized PC              | Test a different MAC address on the secured port and inspect the violation counter | Verified |

### Connectivity Test

A recorded connectivity test from a client to the CSE default gateway (`192.168.20.1`) produced the following result:

- Packets sent: 4
- Packets received: 4
- Packet loss: 0%
- Observed response time: Less than 1 ms

**Result:** The client successfully communicated with its configured default gateway during the recorded test.

### Port-Security Test

The secured switch port was tested with an unauthorized device. The switch's port-security status and violation information were inspected to verify the configured security behavior.

**Result:** The unauthorized MAC address triggered the configured port-security response, and the violation counter provided evidence of the event.

## 9. Project Outcomes

The project demonstrates the practical application of core networking concepts in a simulated college environment.

- Created a logically segmented network for five functional areas.
- Configured VLANs and trunk links for departmental connectivity.
- Implemented inter-VLAN routing using router subinterfaces.
- Configured DHCP relay toward the server network.
- Verified connectivity between clients and their configured gateway.
- Implemented switch port security using sticky MAC learning.
- Tested the configured response to unauthorized MAC addresses.
- Developed a structured network design that can be extended with additional devices and services.

## 10. Future Enhancements

The network can be further improved through the following enhancements:

- **Access Control Lists (ACLs):** Restrict communication between selected departmental VLANs.
- **802.1X Authentication:** Introduce centralized authentication for network access.
- **Redundancy:** Add redundant links and gateway redundancy to improve availability.
- **Monitoring:** Introduce centralized logging and network monitoring.
- **Improved DHCP Management:** Centralize address allocation and document DHCP scopes.
- **Scalability:** Add more departments, wireless access points, and network services.

## 11. Conclusion

The Smart College Department Network demonstrates how a college network can be designed using VLAN-based segmentation, trunk links, router-on-a-stick inter-VLAN routing, DHCP relay, and switch port security.

The implemented configuration was tested in Cisco Packet Tracer. Connectivity testing confirmed successful communication with the CSE default gateway, while the port-security test demonstrated the configured response to an unauthorized MAC address.

The project provides practical experience in network design, configuration, troubleshooting, and access-layer security, forming a foundation for more advanced enterprise network implementations.

## 12. Project Team

**Project Title:** Smart College Department Network

**Subject:** Computer Networks

**Simulation Platform:** Cisco Packet Tracer

**Project Members:**

- Mohammed Zaki
- Meera Thakur
- Kshitij Kadadi
- Manav Jain

## 13. References

1. Cisco Networking Academy — Networking fundamentals and Cisco Packet Tracer learning resources.
2. Cisco documentation — VLAN configuration and IEEE 802.1Q trunking.
3. Cisco documentation — Inter-VLAN routing and router-on-a-stick configuration.
4. Cisco documentation — DHCP relay using `ip helper-address`.
5. Cisco documentation — Switch port security and secure MAC address configuration.

---

**Project Status:** Completed, configured, and tested in Cisco Packet Tracer.

*&#x20;*
