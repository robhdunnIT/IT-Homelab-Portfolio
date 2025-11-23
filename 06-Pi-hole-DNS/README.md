# Project: Centralized DNS & Ad-Blocking (Pi-hole)

**Goal:** To implement network-wide ad blocking and tracking protection while integrating seamless DNS resolution for the local Active Directory domain and extending services to isolated network segments.

**Core Technologies:** Pi-hole (v6 Beta), DNS, DHCP, Conditional Forwarding, Active Directory, Firewall Rules.

---

### Architecture

1.  **Deployment:** Deployed Pi-hole in a lightweight Linux Container (LXC) on Proxmox.
2.  **Network Integration:** Reconfigured the pfSense DHCP server to issue the Pi-hole's IP (`10.0.0.6`) as the primary DNS for all LAN clients.
3.  **Split-Horizon DNS:** Configured Pi-hole to differentiate between public internet queries (forwarded to Google) and local intranet queries (forwarded to the Domain Controller).

---

### Challenges & Solutions

* **Challenge: Active Directory Integration**
    * **Problem:** Standard Pi-hole setups forward all DNS queries to public upstreams. This breaks Active Directory functionality, as public servers cannot resolve local domain names like `dc01.robmox.lan`.
    * **Solution:** Implemented **Conditional Forwarding** (Reverse Server lookup). I configured Pi-hole to identify queries for the `10.0.0.0/24` subnet or the `.lan` domain and forward those specific requests to the local Domain Controller (`10.0.0.5`) instead of the internet.

* **Challenge: Pi-hole v6 Configuration**
    * **Problem:** Utilizing the v6 Beta introduced a new, undocumented syntax for defining upstream servers.
    * **Solution:** Researched and applied the correct syntax by explicitly defining the target port (`#53`), ensuring the internal parser correctly routed local traffic to the DC.

* **Challenge: Cross-VLAN Service Access**
    * **Problem:** The isolated "Guest" VLAN blocked all access to the main LAN, preventing guests from using the Pi-hole.
    * **Solution:** Engineered a "pinhole" in the firewall policy. I created a specific Pass rule allowing DNS traffic (Port 53) to the Pi-hole IP *before* the "Block LAN" rule, and configured Pi-hole to accept queries from non-local subnets.

---

### Outcome

A cleaner, faster network with blocked telemetry and advertisements for all devices (including guests), maintaining full compatibility with local enterprise services.
