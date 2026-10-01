# Lab 01 — Kali Linux VM Deployment

**Status:** Complete
**Date:** 2026-10-01

## Objective

Deploy Kali Linux 2026.2 as a virtual machine on Fedora using QEMU/KVM, libvirt and virt-manager.

The objective was to create the first system in the cybersecurity homelab and verify its virtual hardware, operating system, storage and network connectivity.

---

## Architecture

```text
                    Physical Host
                         │
                         ▼
                   Fedora KDE 44
                         │
                    QEMU / KVM
                         │
                      libvirt
                         │
                Default NAT Network
                 192.168.122.0/24
                         │
                         ▼
                    Kali-2026
                         │
                  192.168.122.195
```

---

## VM Configuration

| Component      | Configuration      |
| -------------- | ------------------ |
| VM name        | Kali-2026          |
| Guest hostname | kali-soc           |
| OS             | Kali Linux 2026.2  |
| RAM            | 8 GiB              |
| vCPU           | 4                  |
| Disk           | 60 GiB             |
| Storage        | VirtIO             |
| Network        | VirtIO             |
| Firmware       | UEFI/OVMF          |
| Chipset        | Q35                |
| Display        | SPICE              |
| Video          | VirtIO             |
| Network mode   | libvirt NAT        |
| IP address     | 192.168.122.195/24 |
| MAC address    | 52:54:00:05:e0:12  |

---

## Technologies

### QEMU/KVM

QEMU/KVM provides the virtualisation backend.

### libvirt

libvirt provides the management framework for the VM and virtual network.

### virt-manager

virt-manager provides the graphical management interface.

### VirtIO

VirtIO is used for efficient virtual storage and networking.

### UEFI/OVMF

The VM boots using virtual UEFI firmware provided by OVMF.

---

## Installation

The Kali Linux installer was booted from the Kali 2026.2 ISO.

The virtual disk was partitioned using the guided partitioning option.

The resulting virtual disk contained:

```text
EFI System Partition
ext4 root filesystem
swap
```

---

## Troubleshooting

### Virtual Secure Boot

The Kali installer initially failed to boot with:

```text
Access Denied -- rejected probably by Secure Boot
```

The virtual UEFI firmware was entered and:

```text
Device Manager
→ Secure Boot Configuration
→ Attempt Secure Boot
```

was disabled.

The Kali installer subsequently booted successfully in UEFI mode.

---

## Verification

### Guest operating system

```bash
hostnamectl
```

Confirmed:

* Kali GNU/Linux Rolling
* KVM virtualisation
* QEMU hardware
* x86-64 architecture

### Kernel

```bash
uname -a
```

Confirmed the Kali kernel was running.

### Network interface

```bash
ip addr
```

Confirmed:

```text
eth0
192.168.122.195/24
```

### Routing

```bash
ip route
```

Confirmed the default gateway:

```text
192.168.122.1
```

---

## Network Tests

### Gateway

```bash
ping -c 4 192.168.122.1
```

Result:

```text
4/4 packets received
0% packet loss
```

### Internet by IP

```bash
ping -c 4 1.1.1.1
```

Result:

```text
4/4 packets received
0% packet loss
```

### DNS and Internet

```bash
ping -c 4 google.com
```

Hostname resolution and connectivity succeeded.

---

## Host-Side Verification

The Fedora host verified the VM using the system libvirt connection.

### VM

```bash
virsh -c qemu:///system list --all
```

```text
Kali-2026    running
```

### Network

```bash
virsh -c qemu:///system net-list --all
```

```text
default    active    yes    yes
```

### Interface

```bash
virsh -c qemu:///system domifaddr Kali-2026
```

```text
vnet0
52:54:00:05:e0:12
192.168.122.195/24
```

The host-side address matched the address reported inside Kali.

---

## Key Learning

This lab demonstrated the complete relationship between the major virtualisation components:

```text
virt-manager
      │
      ▼
   libvirt
      │
      ▼
   QEMU/KVM
      │
      ▼
Kali Linux VM
```

It also demonstrated that virtual networking involves multiple layers:

```text
Kali eth0
   ↓
VM virtual NIC
   ↓
libvirt vnet0
   ↓
libvirt NAT
   ↓
192.168.122.1
   ↓
Host network
   ↓
Internet
```

---

## Evidence

Screenshots for this lab are stored in:

```text
screenshots/
```

Current evidence:

```text
01-kali-desktop.png
02-kali-vm-hardware.png
03-kali-network.png
04-kali-uefi-installer.png
05-kali-baseline.png
06-kali-resources.png
07-kali-network-connectivity.png
08-host-side-vm-network.png
```

If a screenshot was not captured during the original step, it is not recreated solely for documentation purposes.

---

## Lessons Learned

* KVM provides Linux kernel virtualisation.
* QEMU provides virtual hardware.
* libvirt manages virtual machines and networks.
* virt-manager provides a graphical interface to libvirt.
* VirtIO provides efficient virtual devices.
* OVMF provides UEFI firmware for the VM.
* Secure Boot can affect virtual machine booting independently of the physical host.
* libvirt's default network provides NAT connectivity.
* A VM can be verified from both the guest and host perspectives.
* `qemu:///system` identifies the system-wide libvirt instance.
* Troubleshooting requires checking each layer rather than assuming the entire virtualisation stack is broken.

---

## Next Lab

The next phase will investigate the networking stack in greater detail.

Planned topics:

* IPv4 addressing
* Subnets
* DHCP
* DNS
* Routing
* NAT
* TCP and UDP
* Ports
* Sockets
* Network interfaces
* Packet capture
* Nmap
* Wireshark
