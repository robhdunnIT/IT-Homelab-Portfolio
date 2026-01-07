# 04 - Enterprise Storage Solution (TrueNAS Scale)

## Project Overview
Implemeted a Software-Defined Storage (SDS) solution using **TrueNAS Scale** virtualized on Proxmox VE. The goal was to decouple storage management from the hypervisor and create a centralized, network-attached storage repository for backups, ISOs, and file sharing across the `10.0.0.x` subnet.

This project required advanced virtualization techniques, specifically **PCIe/Disk Passthrough**, to allow the TrueNAS VM direct control over physical storage hardware for ZFS integrity.

## Architecture

* **Hypervisor:** Proxmox VE (Node: `robmox`)
* **Storage OS:** TrueNAS Scale (VM ID: `110`)
* **Physical Storage:** 8TB HDD (Seagate Barracuda)
* **File System:** OpenZFS (Pool: `Vault`)
* **Network:** Static IP `10.0.0.10` (Subnet `10.0.0.1/24`)

---

## Technical Challenges & Solutions

### 1. Hardware Passthrough (Disk Controller)
**The Challenge:** Running TrueNAS inside a VM is risky because virtual disks hide the physical drive geometry from ZFS, which endangers data integrity during power loss.
**The Solution:** I used the Proxmox Shell to identify the unique disk ID and pass the physical raw device directly to the VM, bypassing the hypervisor's virtual disk layer.

**Implementation Command:**
```bash
# Identifying the unique disk ID
ls -l /dev/disk/by-id/

# Mapping the physical 8TB drive to the VM (SCSI Controller 2)
qm set 101 -scsi2 /dev/disk/by-id/ata-ST8000DM004-2CX188_ZCT2M080
