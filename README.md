## 📌Project Overview

This lab is a self-contained, three-VM security environment built entirely in VirtualBox. The goal is to simulate a realistic attack-and-defend scenario: one machine acts as a vulnerable target, one acts as the attacker, and one serves as the monitoring and detection hub. All traffic is isolated on an internal network with no exposure to the host machine or the wider network.

## SOC lab Topology

<img src="Homelab-images/SOC architecture v1.png" width=600 height=600>

## Step 1: Setting up Virtual Machines

### 1.1 Installing Ubuntu server

1. Download **Ubuntu Server ISO** from [Ubuntu Official Wesbite](https://ubuntu.com/download/server)
2. Create a new VM in your preferred virtualisation software (VMware/VirtualBox):

- **Memory (RAM):** 4-6GB
- **CPU:** 2-4 CPU cores
- **Virtual Hard Disk:** 50GB
- **Network:** Set temporarily to NAT

3. Boot the VM and complete the Ubuntu Server installation

### 1.2 Installing Windows 10 VM

1. Download **Windows 10 installation media** [Windows Official Website](https://www.microsoft.com/en-us/software-download/windows10)
2. Create a new VM in your preferred virtualisation software:

- **Memory (RAM):** 2-4GM RAM
- **CPU:** 2 CPU cores
- **Virtual Hard Disk:** 50GB
- **Network:** Set temporarily to NAT

3. Boot the VM and complete the Windows installation

### 1.3 Installting Kali Linux VM

1. Download **Kali Linux** [Kali Linux Official Website](https://www.kali.org/get-kali/#kali-virtual-machines)
2. Extrand the downloaded Kali **.7z** file
3. Double-click the **.vbox** file inside - this-auto imports Kali into VirtualBox :

- **Network:** Set temporarily to NAT
- Default credentials are : username **kali**, password **kali**
