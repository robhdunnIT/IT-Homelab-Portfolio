# IT-Homelab-Portfolio
I’m currently building an enterprise-style homelab environment to strengthen my skills in virtualization, networking, system automation, and cybersecurity fundamentals.
This repository includes detailed write-ups of my configurations, troubleshooting steps, and project builds as I apply industry best practices in a controlled lab environment.

Core Technologies

    Virtualization: Proxmox VE

    Networking: pfSense, VLANs (802.1q), Subnetting, PoE, Policy-Based Routing

    Controllers: Omada Software Controller

    Hardware: Dell OptiPlex, Intel Server NICs (i350-T2), TP-Link Managed Switches

    OS/Containers: Linux (Ubuntu, Debian), Windows Server, LXC

Lab Architecture (v1.0)

My current lab is built on a single, power-efficient Dell OptiPlex server running Proxmox. The core of my network is a virtualized pfSense router that handles all traffic, DHCP, and DNS.

This setup uses an Intel i350-T2 NIC passed directly to the pfSense VM for maximum performance, with separate ports for WAN (Internet) and LAN (internal network). This allows me to create multiple, isolated VLANs for my projects (like a secure lab, an IoT network, and a main trusted network) that are completely separate from my home's main network.

Projects

Here are the step-by-step reports on the major services I've deployed and the problems I've solved.

    [01 - Virtualized pfSense Firewall](./01-pfSense-Firewall/README.md)

        Description: Deployed an enterprise-grade pfSense firewall as a virtual machine, replacing my consumer ISP router. This project involved PCI passthrough, solving a "Double NAT" IP conflict, and troubleshooting a VM performance bottleneck to achieve full gigabit speeds.

    [02 - Enterprise Wi-Fi Deployment](./02-Omada-Controller/README.md)

        Description: Deployed a TP-Link Omada (EAP245) enterprise access point. This project involved setting up a lightweight Ubuntu container to run the Omada Software Controller, troubleshooting "stuck" package dependencies in Linux, and using a factory reset to solve a "failed adoption" state.
