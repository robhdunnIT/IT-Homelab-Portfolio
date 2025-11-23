# My IT & Cybersecurity Homelab Portfolio

Hello! I'm an IT and Cybersecurity student passionate about building, securing, and documenting real-world systems from the ground up. This portfolio is a living document of my hands-on projects, demonstrating my skills in virtualization, advanced networking, and systems administration.

---

## Core Technologies

* **Virtualization:** Proxmox VE
* **Networking:** pfSense, VLANs (802.1q), Subnetting, PoE, Policy-Based Routing
* **Controllers:** Omada Software Controller
* **Hardware:** Dell OptiPlex, Intel Server NICs (i350-T2), TP-Link Managed Switches
* **OS/Containers:** Linux (Ubuntu, Debian), Windows Server, LXC

---

## Lab Architecture (v1.0)

My current lab is built on a single, power-efficient Dell OptiPlex server running Proxmox. The core of my network is a virtualized pfSense router that handles all traffic, DHCP, and DNS.

This setup uses an Intel i350-T2 NIC passed directly to the pfSense VM for maximum performance, with separate ports for WAN (Internet) and LAN (internal network). This allows me to create multiple, isolated VLANs for my projects (like a secure lab, an IoT network, and a main trusted network) that are completely separate from my home's main network.



---

## Projects

Here are the step-by-step reports on the major services I've deployed and the problems I've solved.

* **[01 - Virtualized pfSense Firewall](./01-pfSense-Firewall/README.md)**
* **[02 - Enterprise Wi-Fi Deployment](./02-Omada-Controller/README.md)**
* **[03 - Active Directory Domain Lab](./03-Active-Directory-Lab/README.md)**
* **[04 - Secure VPN Services](./04-VPN-Services/README.md)** *(Coming Soon)*
* ** [05 - Guest Network Isolation](./05-Guest-Network-Isolation/README.md)**
