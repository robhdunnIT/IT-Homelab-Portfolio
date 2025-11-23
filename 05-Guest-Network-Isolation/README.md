# Project: Guest Network Segmentation & Isolation

**Goal:** Designed and deployed a secure, isolated network segment for guest usage, ensuring untrusted devices have internet access without compromising the core homelab infrastructure.

**Core Technologies:** pfSense, VLANs (802.1q), Switch PVID/Tagging, Firewall Rules, Hardware Reuse.

---

### Architecture

1.  **Network Design:** Created a dedicated VLAN (`20`) and Subnet (`10.0.20.0/24`) in pfSense to segment traffic at Layer 3.
2.  **Physical Isolation:** Configured specific ports on the managed switch as Access Ports for VLAN 20 using **PVID tagging**. Any device plugged into these ports is automatically forced onto the Guest network before traffic ever reaches the router.
3.  **Hardware:** Repurposed a consumer router into **Access Point (AP) Mode** to serve as the physical wireless interface for the isolated network.

---

### Challenges & Solutions

* **Challenge: Layer 2 Segmentation (Tagging)**
    * **Problem:** The switch initially allowed traffic to leak or failed to pass tags to the unmanaged AP.
    * **Solution:** Configured **802.1Q PVID** settings on the switch port. This ensures untagged traffic coming from the "dumb" AP is immediately tagged as "VLAN 20" by the switch logic before entering the network backbone.

* **Challenge: Firewall Logic & Rule Matching**
    * **Problem:** Guests could connect via DHCP but had no internet access. Firewall logs showed valid packets hitting the "Default Deny" rule at the bottom of the stack.
    * **Solution:** The firewall rule Source definitions were too restrictive. I modified the outbound "Pass" rule to accept **Source: Any** on the Guest Interface. This successfully allowed traffic to flow to the WAN gateway while still being subject to the strict "Block LAN" rules placed above it.

---

### Outcome

A fully isolated network segment. Guests have full internet access but are cryptographically blocked from accessing the management subnet or any internal servers.
