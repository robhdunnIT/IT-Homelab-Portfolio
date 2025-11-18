# Project: Virtual Active Directory (AD) Lab

**Goal:** To build a foundational enterprise environment using Active Directory. This project involved deploying a Windows Server 2022 Domain Controller and a Windows 11 Enterprise client, configuring them to communicate, and troubleshooting a complex domain-join failure.

**Core Technologies:** Proxmox VE, Windows Server 2022, Windows 11 Enterprise, Active Directory Domain Services (AD DS), DNS, Group Policy.

---

### Process & Deployment

1.  **Server (DC01):** Deployed a Windows Server 2022 VM, assigned it a static IP, and installed the **AD DS** and **DNS Server** roles.
2.  **Forest:** Promoted the server to a new domain controller, creating a new forest (e.g., `mylab.local`).
3.  **Client (PC01):** Deployed a Windows 11 Enterprise VM, ensuring it met **UEFI, Secure Boot, and TPM 2.0** requirements within Proxmox.

---

### Challenges & Solutions

* **Challenge: Windows 11 VM Requirements**
    * **Problem:** The Windows 11 installer blocked installation, requiring a virtual TPM 2.0 and Secure Boot.
    * **Solution:** In Proxmox, I reconfigured the VM to use **OVMF (UEFI)**, then added a virtual **EFI Disk** (with Secure Boot enabled) and a virtual **TPM State 2.0** device. This satisfied the installer's requirements.

* **Challenge: Client Domain Join Failure ("NTLM authentication disabled")**
    * **Problem:** The client PC failed to join the domain, citing an NTLM error. This indicated a Kerberos authentication failure.
    * **Solution (Multi-Step):**
        1.  **DNS Isolation:** I reconfigured the client's network adapter to use *only* the Domain Controller for DNS, preventing it from sending domain queries to the router.
        2.  **Stale Account Removal:** On the Domain Controller, I used "Active Directory Users and Computers" to find and delete the stale computer account for `PC01`, which was corrupted from the first failed attempt.
        3.  **Forced Disjoin & Re-Identity:** On the client, I used PowerShell (`Remove-Computer`) to forcibly remove the broken trust. I then **renamed the PC** to `W11-CLIENT-01` to ensure a 100% fresh identity before successfully re-joining the domain.

---

### Outcome

A fully functional Active Directory environment with a trusted Domain Controller (`DC01`) and a domain-joined client (`W11-CLIENT-01`). This lab is now the base for all future enterprise projects, such as testing Group Policy Objects (GPOs), managing user accounts, and deploying services.
