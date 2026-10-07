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
| Network Interface | VirtIO                |
| Hypervisor        | QEMU/KVM              |
| Network           | libvirt `default` NAT |

---

# Virtual Network

The current Kali VM uses the default libvirt NAT network.

```text
                    Internet
                       │
                       │
                Fedora Host
                       │
                 QEMU / KVM
                       │
                  libvirt NAT
                 192.168.122.0/24
                       │
                       │
                 Kali-2026 VM
                       │
              192.168.122.195
```

### Current Network Configuration

| Component    | Address            |
| ------------ | ------------------ |
| Network      | `192.168.122.0/24` |
| Gateway      | `192.168.122.1`    |
| Kali VM      | `192.168.122.195`  |
| Network Type | NAT                |
| Management   | libvirt            |

The default NAT network provides controlled internet connectivity for the Kali VM without exposing the VM directly to the physical network.

Future vulnerable target machines will use a **separate isolated virtual network** rather than the default NAT network. This will allow offensive-security exercises to be performed within a controlled lab environment.

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

* [ ] IP addressing
* [ ] Subnetting
* [ ] DHCP
* [ ] DNS
* [ ] Routing
* [ ] NAT
* [ ] TCP/IP
* [ ] UDP
* [ ] Ports
* [ ] Sockets
* [ ] `ip`
* [ ] `ss`
* [ ] `ping`
* [ ] `traceroute`
* [ ] `tcpdump`
* [ ] Wireshark
* [ ] Nmap

## Phase 3 — Offensive Security

* [ ] Deploy vulnerable Linux target
* [ ] Create isolated attack network
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
* [ ] Bleder security testing
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
├── journal/
│   └── YYYY-MM-DD.md
│
└── labs/
    └── 01-kali-vm/
        ├── README.md
        ├── screenshots/
        └── notes/
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

**Current focus:** Networking fundamentals and continued development of the cybersecurity homelab.

The next major objective is to develop a stronger understanding of networking before introducing vulnerable targets and offensive-security scenarios.
