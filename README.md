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

## Step 2 : Setting up Wazuh

### 2.1 Install Wazuh Server on Ubuntu (While on NAT)

1. Download the Wazuh installation assistant :

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
```

2. Run this all-in-one script which installs the Manager, Indexer and Dashboard automatically :

```bash
sudo bash ./wazuh-install.sh -a
```

> **Note:** If it prompts you anything about the required specs you can re-run the script by adding the -i flag at the end which ignores the requirment check

3. Save the printed admin credentials immediately after the installation is done - you won't see them again
4. Verify the services are running:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

### 2.2 Install Sysmon & Wazuh Agent on Windows 10 (While on NAT)

### 2.2.1 Install Sysmon

1. Download the **Pre-generated Balanced** configuration made by **Olaf Hartong** [Olaf Harton's Sysmon config](https://github.com/olafhartong/sysmon-modular#pre-generated-configurations)
2. Download & Extract **Sysmon** from [Sysmon Official Website](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
3. Copy the downloaded **sysmonconfig.xml** into that folder
4. Open PowerShell as Administrator in the Sysmon folder and run:

```bash
.\Sysmon64.exe -accepteula -i .\sysmonconfig.xml
```

5. Verify it's running:

```bash
Get-Service Sysmon64
```

### 2.2.2 Install Wazuh Agent

1. Download the **Wazuh Windows Agent** MSI [Wazuh Windows Agent](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html)
2. When prompted for the **Wazuh Manager IP**, enter 192.168.10.10 (the static IP Ubuntu Server will use later)
3. Start the agent service from Powershell as Administrator:

```bash
NET START WazuhSvc
```

### 2.3 Install Wazuh Agent on Kali Linux

> **Note:** You need root user priviliges to run all these commands

1. Install the following packages if missing:

```bash
apt-get install gnupg apt-transport-https curl
```

2. Install the GPG key:

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg
```

3. Add the repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
```

4. Update the package information:

```bash
sudo apt update && sudo apt install wazuh-agent -y
```

5. Configure it to point to your Wazuh Manager:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Find the <server> block and set:

```xml
<address>192.168.10.10</address>
```

6. Start and enable the agent:

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```
