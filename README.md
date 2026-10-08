## 📌Project Overview
This lab is a self-contained, three-VM security environment built entirely in VirtualBox. The goal is to simulate a realistic attack-and-defend scenario: one machine acts as a vulnerable target, one acts as the attacker, and one serves as the monitoring and detection hub. All traffic is isolated on an internal network with no exposure to the host machine or the wider network.

## Network Topology
+----------------------+        +----------------------+
|   Kali Linux VM      |        |   Windows 10 VM      |
|   "The Attacker"     | -----> |   "The Victim"       |
|   192.168.10.20      |        |   192.168.10.30      |
|                      |        |   + Wazuh Agent      |
|                      |        |   + Sysmon           |
+----------------------+        +----------------------+
                                          |
                               (agent streams telemetry)
                                          |
                                          ↓
                        +--------------------------------+
                        |   Ubuntu Server VM             |
                        |   "The Monitor"                |
                        |   192.168.10.10                |
                        |                                |
                        |   Wazuh Manager                |
                        |   Wazuh Indexer (OpenSearch)   |
                        |   Wazuh Dashboard              |
                        +--------------------------------+
                                          ↑
                             (accessed via browser on host
                              at https://192.168.10.10)
