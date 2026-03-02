![Raspberry Pi](https://img.shields.io/badge/Hardware-Raspberry_Pi_5-C51A4A?logo=raspberry-pi&logoColor=white)
![VPN](https://img.shields.io/badge/VPN-WireGuard-881717?logo=wireguard&logoColor=white)
![Pi-hole](https://img.shields.io/badge/DNS-Pi--hole-96060C?logo=pi-hole&logoColor=white)
![Mikrotik](https://img.shields.io/badge/Router-Mikrotik-004A99?logo=mikrotik&logoColor=white)
# 🛡️ Security-First Home Lab Infrastructure

## Philosophy: Defense in Depth
My home network is built on the principle of **Defense in Depth**. Enforcing traffic isolation, and minimizing exposure through proactive DNS filtering. This project serves as my sandbox for testing security configurations and monitoring network traffic.

## 🏗️ Architecture Overview
![Network Diagram](https://github.com/ManolisCraftedTech/Home-Network/blob/main/%CE%97%CE%9F%CE%9C%CE%95%20%CE%9D%CE%95%CE%A4.drawio.png)

The infrastructure is segmented to ensure that untrusted devices (IoT) cannot communicate laterally with trusted workstations (LAN).

| Zone | Subnet | Role |
| :--- | :--- | :--- |
| **Trusted (LAN)** | `192.168.20.0/24` | Development & Sensitive Tasks |
| **Wireless/Mobile**| `10.10.10.0/24` | Daily drivers (Tablets, Phones) |
| **IoT Segments** | `10.10.10.0/24` | Isolated Smart Devices |

### Core Security Controls
* **Traffic Isolation:** Firewall rules on the Mikrotik Routerboard prevent unauthorized access between the IoT and LAN segments.
* **DNS Sinkholing:** A [Raspberry PI](https://github.com/ManolisCraftedTech/RaspberryDNS) running **Pi-hole** acts as the central DNS provider. This blocks telemetry/tracking at the network level and provides a layer of protection against malicious domains.
* **Visibility:** Continuous monitoring of DNS queries to identify unusual traffic patterns.

---
---
