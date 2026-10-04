# 01: Virtualized pfSense Firewall

[← Back to portfolio](../README.md) · [Next: Omada Wi-Fi →](../02-Omada-Controller/README.md)

> **Summary:** Replaced a consumer ISP router with a virtualized pfSense firewall on Proxmox, using a dedicated passthrough NIC. It's the routing, DHCP, and firewall foundation for every other project in this lab.

**Technologies:** Proxmox VE · pfSense CE · PCI passthrough · Intel i350-T2 · subnetting · system tunables

---

## Build

1. **Hardware:** Installed a dual-port Intel i350-T2 NIC in the Proxmox server.
2. **Virtualization:** Created a pfSense VM and used Proxmox **PCI passthrough** so the VM gets exclusive, direct control of the NIC, with no virtual switching in the data path.
3. **Interfaces:** Assigned one port as **WAN** (to the ISP modem) and the other as **LAN** (to the managed switch).

```mermaid
flowchart LR
    ISP["ISP modem"] -->|"WAN: i350 port 1"| PF["pfSense VM"]
    PF -->|"LAN: i350 port 2<br/>10.0.0.0/24"| SW["TL-SG1016PE switch"]
```

---

## Challenges & Solutions

### 1. Subnet conflict with the ISP router (double NAT)

- **Problem:** The ISP modem is also a router, so it handed the pfSense WAN port a private `192.168.x.x` address. That overlapped with pfSense's default LAN network (`192.168.1.1`), so routing between the two broke.
- **Fix:** Moved the whole lab to a non-overlapping RFC 1918 subnet, **`10.0.0.0/24`**. The conflict went away right away, and the lab now sits cleanly behind the home network as its own segment.

### 2. Gigabit connection capped at ~600 Mbps

- **Problem:** Speed tests topped out around 600 Mbps on a gigabit plan.
- **Diagnosis:** Traced it to **hardware offloading** (TCP Segmentation Offload and Large Receive Offload), a common source of pfSense throughput problems, especially in virtualized setups.
- **Fix:** Added these to **System → Advanced → System Tunables** to turn the features off at the driver level:

  ```text
  net.inet.tcp.tso = 0
  net.inet.tcp.lro = 0
  ```

  Throughput went back to **900+ Mbps**.

---

## Outcome

A stable, full-speed router and firewall for the whole lab. Later projects build on it: VLAN segmentation ([05](../05-Guest-Network-Isolation/README.md)) and DNS integration ([06](../06-Pi-hole-DNS/README.md)).

**Skills demonstrated:** PCI passthrough · WAN/LAN design · RFC 1918 subnet planning · NAT · performance troubleshooting
