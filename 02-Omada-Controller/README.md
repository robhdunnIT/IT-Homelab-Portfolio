# Project: Enterprise Wi-Fi Deployment (Omada)

**Goal:** To deploy a controller-managed, PoE-powered enterprise access point to provide secure, high-performance Wi-Fi for my lab.

**Core Technologies:** Proxmox VE, LXC (Linux Containers), TP-Link Omada, PoE, VLANs, 802.1q.

---

### Process & Deployment

1.  **Controller:** Deployed the Omada Software Controller inside a lightweight **Ubuntu LXC container** on Proxmox, giving it a static IP (`10.0.0.3`) for 24/7 management.
2.  **Hardware:** Installed an **Omada EAP245** access point, powered by my `TP-SG1016PE` PoE switch.

---

### Challenges & Solutions

* **Challenge 1: Failed Installation**
    * **Problem:** Manually installing the controller software on Ubuntu 22.04 failed due to complex, broken package dependencies (`mongodb`, `libssl1.1`).
    * **Solution:** I researched the issue and found a community-made "Easy Install Script." This script automated the entire installation, correctly added the necessary repositories, and installed all dependencies, solving the problem in a clean and repeatable way.

* **Challenge 2: Failed AP Adoption**
    * **Problem:** The Omada controller wizard could not discover the EAP245, even though it was on the same network.
    * **Solution:** I diagnosed that the AP was not in a factory-default state. I performed a **physical factory reset** (holding the reset pinhole for 15s), which forced the AP into "Pending Adoption" mode. The controller was then able to discover and adopt it immediately.

---

### Outcome

A fully functional, controller-managed Wi-Fi network. This system is now ready to be segmented into multiple SSIDs, each tagged with a different VLAN (e.g., `GUEST`, `IOT`, `LAB`) for advanced network security.
