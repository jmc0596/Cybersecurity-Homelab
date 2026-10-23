# Cybersecurity Homelab

## Overview

This repository documents my personal cybersecurity homelab, built to support my **BSc Cyber Security studies** through practical, hands-on learning.

The lab provides an environment where I can safely practise Linux administration, virtualisation, networking, offensive security, defensive security, monitoring, automation, web security, and incident investigation.

The goal is to connect theoretical knowledge with practical skills and produce documented evidence of my technical development.

---

## Learning Methodology

My approach to the homelab follows this cycle:

**LEARN → BUILD → TEST → TROUBLESHOOT → CAPTURE EVIDENCE → DOCUMENT → COMMIT → PUSH → REFLECT**

Each lab is intended to demonstrate not only that something was configured successfully, but also that I understand:

* What I configured
* Why I configured it
* How the technology works
* How I tested it
* Problems encountered
* How problems were resolved
* Security implications
* What I learned
* How the knowledge can be applied professionally

---

# Host Environment

| Component                 | Configuration         |
| ------------------------- | --------------------- |
| Operating System          | Fedora KDE 44         |
| Architecture              | x86-64                |
| CPU                       | 16 logical processors |
| RAM                       | 30 GiB                |
| CPU Virtualisation        | AMD-V                 |
| Virtualisation Backend    | QEMU/KVM              |
| Virtualisation Management | libvirt               |
| Graphical Management      | virt-manager          |
| Firmware                  | UEFI / OVMF           |
| Physical Secure Boot      | Disabled              |

The Fedora system is my primary operating system.

---

# Virtual Machines

| VM        | Purpose                                     | Status  |
| --------- | ------------------------------------------- | ------- |
| Kali-2026 | Security testing and cybersecurity learning | Running |

### Kali-2026

The Kali Linux virtual machine is used as the primary security-testing environment.

| Component         | Configuration         |
| ----------------- | --------------------- |
| Operating System  | Kali Linux Rolling    |
| Hostname          | `kali-soc`            |
| RAM               | 8 GiB                 |
| vCPU              | 4                     |
| Virtual Disk      | 60 GiB                |
| Firmware          | UEFI / OVMF           |
| Machine Type      | Q35                   |
| Disk Interface    | VirtIO                |
| NICs              | 2 × VirtIO            |
| Hypervisor        | QEMU/KVM              |
| Networks          | `lab-isolated` + `default` NAT |

---

# Virtual Network

The Kali VM has two virtual network adapters: one on the libvirt `default` NAT network for internet access, and one on the isolated `lab-isolated` network for lab exercises.

```text
                         Internet
                            │
                       Fedora Host
                            │
                        QEMU / KVM
               ┌────────────┴────────────┐
               │                         │
         libvirt NAT               lab-isolated
      virbr0 192.168.122.0/24    virbr10 10.10.10.0/24
       (forwarding: NAT)         (no forwarding)
               │                         │
               └──────┐           ┌──────┘
                      │           │
                 eth1 .130    eth0 .101
                      └─ Kali-2026 ─┘
```

### Current Network Configuration

| Interface | Network        | Address              | Gateway / Bridge | Purpose                |
| --------- | -------------- | -------------------- | ---------------- | ---------------------- |
| `eth1`    | `default` NAT  | `192.168.122.130/24` | `192.168.122.1`  | Internet access (default route) |
| `eth0`    | `lab-isolated` | `10.10.10.101/24`    | `10.10.10.1`     | Isolated lab exercises |

The default NAT network provides controlled internet connectivity for the Kali VM without exposing the VM directly to the physical network.

The isolated `lab-isolated` network (Lab 03) has no forwarding, so guests on it have no route to the internet or the home network. Future vulnerable target machines will be attached only to this network. Before exercises with vulnerable targets, Kali's NAT adapter should be disconnected or IP forwarding on Kali confirmed as disabled.

---

# Virtualisation Architecture

The homelab uses several layers of virtualisation technology:

```text
Physical Hardware
       │
       ▼
Fedora Linux Host
       │
       ▼
QEMU/KVM
       │
       ▼
libvirt
       │
       ▼
virt-manager
       │
       ▼
Kali Linux Virtual Machine
```

### Technologies

**KVM**

Kernel-based Virtual Machine provides hardware-assisted virtualisation through the Linux kernel.

**QEMU**

Provides the virtual hardware and machine emulation used by the virtual machine.

**libvirt**

Provides a management layer for virtual machines, storage, and virtual networks.

**virt-manager**

Provides a graphical interface for managing the libvirt virtualisation environment.

Professional description:

> I am using QEMU/KVM as the virtualisation backend, managed through libvirt, with virt-manager as the graphical management interface.

---

# Homelab Roadmap

## Phase 1 — Infrastructure

* [x] Fedora Linux host
* [x] QEMU/KVM
* [x] libvirt
* [x] virt-manager
* [x] Virtual networking
* [x] NAT networking
* [x] Kali Linux VM
* [x] UEFI / OVMF
* [x] VirtIO devices
* [x] VM storage
* [x] VM network verification

## Phase 2 — Networking

* [x] IP addressing
* [x] Subnetting
* [x] DHCP
* [x] DNS
* [x] Routing
* [x] NAT
* [ ] TCP/IP
* [ ] UDP
* [ ] Ports
* [ ] Sockets
* [x] `ip`
* [ ] `ss`
* [x] `ping`
* [ ] `traceroute`
* [ ] `tcpdump`
* [ ] Wireshark
* [ ] Nmap

## Phase 3 — Offensive Security

* [ ] Deploy vulnerable Linux target
* [x] Create isolated attack network
* [ ] Network discovery
* [ ] Port scanning
* [ ] Service enumeration
* [ ] Vulnerability identification
* [ ] Controlled exploitation
* [ ] Privilege escalation
* [ ] Post-exploitation analysis
* [ ] Evidence collection

## Phase 4 — Defensive Security / SOC

* [ ] Linux security logs
* [ ] Network monitoring
* [ ] Packet capture
* [ ] Suricata
* [ ] IDS alerts
* [ ] Detection rules
* [ ] Alert investigation
* [ ] Incident timelines
* [ ] SIEM concepts
* [ ] Security investigations

## Phase 5 — Automation

* [ ] Bash scripting
* [ ] Python security tools
* [ ] File hashing
* [ ] Network utilities
* [ ] Log analysis
* [ ] Security automation
* [ ] Scheduled tasks

## Phase 6 — Web Security

* [ ] HTTP fundamentals
* [ ] HTTPS
* [ ] HTTP request analysis
* [ ] HTTP response analysis
* [ ] Web reconnaissance
* [ ] Vulnerability testing
* [ ] Web application security
* [ ] Security assessment documentation

---

# Labs

## Lab 01 — Kali Linux VM Deployment

**Status:** Complete

This lab documents the deployment and verification of the Kali Linux virtual machine.

Topics covered:

* QEMU/KVM
* libvirt
* virt-manager
* UEFI / OVMF
* Q35 machine type
* VirtIO
* Virtual disks
* Virtual networking
* NAT
* Kali Linux installation
* Virtual Secure Boot troubleshooting
* IP addressing
* DNS
* Network connectivity
* Host-side VM verification
* libvirt connection scopes

Documentation:

`labs/01-kali-vm/README.md`

## Lab 02 — Network Fundamentals

**Status:** Complete

This lab investigates the Kali VM's network configuration on the libvirt `default` NAT network.

Topics covered:

* IPv4 addressing and subnetting
* Routing tables and the default route
* DNS configuration and resolution
* Gateway and external connectivity testing
* The host-side libvirt NAT network

Documentation:

`labs/02-network-fundamentals/README.md`

## Lab 03 — Isolated Virtual Network

**Status:** Complete

This lab creates the `lab-isolated` libvirt network with no forwarding and tests that Kali has no route out of it. It was then extended with a second NAT adapter so Kali has internet access while staying attached to the isolated network.

Topics covered:

* libvirt network XML definitions
* Creating and managing networks with `virsh`
* NAT versus isolated networks
* Host bridge and DHCP lease verification
* Isolation testing and routing analysis
* Multiple virtual NICs and NetworkManager connection profiles

Documentation:

`labs/03-isolated-network/README.md`

`labs/03-isolated-network/dual-nic-recovery.md`

---

# Evidence

Screenshots and other evidence are stored alongside the relevant lab documentation.

Evidence is used to demonstrate that configurations and tests were actually performed rather than simply describing them retrospectively.

Examples include:

* VM configuration
* Kali desktop
* Network configuration
* Installation process
* System resources
* Connectivity tests
* Host-side libvirt verification
* Troubleshooting results

---

# Repository Structure

```text
homelab/
├── README.md
├── .gitignore
├── journal/
│   └── YYYY-MM-DD.md
│
└── labs/
    ├── 01-kali-vm/
    │   ├── README.md
    │   ├── screenshots/
    │   └── notes/
    ├── 02-network-fundamentals/
    │   ├── README.md
    │   └── screenshots/
    └── 03-isolated-network/
        ├── README.md
        ├── dual-nic-recovery.md
        ├── config/
        └── screenshots/
```

As the homelab develops, additional labs will be added using the same structure.

---

# Documentation and Git Workflow

Git is used to maintain a history of the homelab's development.

The workflow is:

```text
Work in the lab
      │
      ▼
Capture evidence
      │
      ▼
Document the work
      │
      ▼
git status
      │
      ▼
git add
      │
      ▼
git diff --cached
      │
      ▼
git commit
      │
      ▼
git push
      │
      ▼
GitHub
```

Commits should describe meaningful changes to the project rather than individual commands.

Example:

```bash
git commit -m "Document Kali VM deployment and network verification"
```

---

# Learning Goals

The long-term goal of this homelab is to develop practical cybersecurity skills that complement my academic studies.

Areas of development include:

* Linux administration
* Virtualisation
* Networking
* Network security
* Offensive security
* Defensive security
* SOC operations
* Security monitoring
* Incident investigation
* Python
* Bash
* Automation
* Web application security
* Vulnerability assessment
* Technical documentation
* Troubleshooting

The homelab will evolve alongside my BSc Cyber Security studies and will provide a practical environment for applying concepts learned through university coursework.

---

# Current Status

**Phase 1 — Infrastructure: Complete**

**Phase 2 — Networking: In progress** (Labs 02 and 03 complete)

**Current focus:** Creating an Ubuntu target VM on the `lab-isolated` network.

The next major objective is to carry out controlled network discovery and port-scanning exercises between Kali and the target, entirely within the isolated lab network.
