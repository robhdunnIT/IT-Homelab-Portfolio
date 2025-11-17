# Project: Virtualized Enterprise Firewall (pfSense)

**Goal:** To replace my consumer-grade ISP router with a robust, high-performance, virtualized pfSense firewall. This created a secure, configurable network foundation for my entire lab.

**Core Technologies:** Proxmox VE, pfSense CE, PCI Passthrough, Intel i350-T2 NIC, Network Segmentation, System Tunables.

---

### Process & Deployment

1.  **Hardware Prep:** Installed a dual-port Intel i350-T2 NIC in my Proxmox server.
2.  **Virtualization:** Created a new pfSense VM and used Proxmox's **PCI Passthrough** feature to give the VM exclusive, direct control over the NIC for maximum performance.
3.  **Configuration:** Configured one port as **WAN** (to the modem) and the other as **LAN** (to my managed switch).

---

### Challenges & Solutions

This project was an excellent real-world troubleshooting exercise:

* **Challenge 1: IP Conflict (Double NAT)**
    * **Problem:** The ISP modem was also a router, issuing a private IP (`192.168.x.x`) to my pfSense WAN port. This conflicted with the default pfSense LAN network (`192.168.1.1`).
    * **Solution:** I re-architected my entire lab onto a new, non-conflicting RFC1918 subnet (`10.0.0.0/24`). This immediately resolved the conflict and properly isolated my lab from the main home network.

* **Challenge 2: Performance Bottleneck (Slow Speed)**
    * **Problem:** After installation, speed tests were capped at ~600Mbps, far below my gigabit plan.
    * **Solution:** I diagnosed this as a **Hardware Offloading** conflict, a common issue with virtualized NICs. By adding `net.inet.tcp.tso=0` and `net.inet.tcp.lro=0` to the System Tunables, I disabled the problematic features at the driver level, which resolved the bottleneck and restored my full 900+ Mbps speeds.

---

### Outcome

A stable, high-performance router running my entire network. This build is now the secure foundation for all future projects, such as creating isolated VLANs for security labs and implementing VPNs.
