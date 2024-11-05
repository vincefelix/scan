# README

## Objective

The mission is to access a target machine on the internal network named `01_scan_RRF-CONTROL`. The only entry point available is through the machine `01_scan_laptop`. By connecting to "01_scan_laptop", we can identify and scan machines on the internal network, find an open port on `01_scan_RRF-CONTROL`, and establish an SSH connection to obtain the `RRF-CONTROL>` prompt.

## Prerequisites

- SSH access to "01_scan_laptop" on port `10122`.
- Password: a single space (`" "`).

---

## Step-by-Step Procedure

### 1. SSH Connection to the Entry Machine

```bash
ssh -p 10122 root@localhost
```

Explanation: This command initiates an SSH connection to "01_scan_laptop" using the specified port 10122 (set up for local port forwarding). This grants access to the starting machine within the internal network.
Note: The password for this SSH connection is a single space (" ").
### 2. Scanning the Local Network with arp-scan

```bash
arp-scan --interface=enp0s8 --localnet
```
Explanation: arp-scan is used to identify machines connected to the same local network (LAN) by scanning all IP addresses on this network. It uses the enp0s8 network interface (configured for the internal network) and displays IP and MAC addresses of connected devices.
Result: This command reveals the IP address 192.168.23.42, which corresponds to the target machine 01_scan_RRF-CONTROL.
### 3. Checking Network Interfaces
```bash
ip a
```
Explanation: The ip a command displays detailed information about the machine's network interfaces, including assigned IP addresses for each interface.
Purpose: This allows us to confirm that the enp0s8 interface is connected to the internal network and uses an IP address compatible with 192.168.23.42.
### 4. Port Scan on the Target Machine with nmap
```bash
nmap -T4 -p- 192.168.23.42
```
Explanation: nmap is a network scanning tool used to detect open ports and active services. Here, -p- scans all ports (from 1 to 65535), and -T4 sets a faster scan speed.
Result: The scan shows that port 4223 is open on 192.168.23.42. This port could potentially be used for SSH access.
### 5. SSH Connection to the Target Machine
```bash
ssh -p 4223 root@192.168.23.42
```
Explanation: After identifying port 4223 as open, this SSH command attempts a direct connection to 192.168.23.42 on this port as the root user.
Expected Result: Successfully connecting to this machine and seeing the RRF-CONTROL> prompt confirms that access to the target server 01_scan_RRF-CONTROL has been obtained.
Explanation of Tools Used
arp-scan
text
- Description: `arp-scan` is a network scanning tool that sends ARP (Address Resolution Protocol) requests to identify devices on the local network.
- Key Options:
  - `--interface=<interface>`: Specifies the network interface to use for the scan.
  - `--localnet`: Scans all IP addresses in the local network.
- Usage: Useful for identifying IP and MAC addresses of active devices on a local network, ideal for discovering machines without prior knowledge of their IPs.
nmap
text
- Description: `nmap` is a widely used network scanning tool for discovering hosts and services on a network.
- Key Options:
  - `-T4`: Sets the scan speed (from `T0` to `T5`), with `T4` being fast but still fairly accurate.
  - `-p-`: Scans all ports, from 1 to 65535.
- Usage: `nmap` is valuable for identifying open ports and associated services on a target machine, making it essential for locating entry points.
## Conclusion
These steps allow us to:

Access the starting machine 01_scan_laptop.
Identify the target machine 01_scan_RRF-CONTROL on the internal network.
Scan the target machine for an open port.
Successfully connect to the target server via SSH and obtain the RRF-CONTROL> prompt.
These commands and tools are essential for exploring a local network in a secure and efficient way, discovering connected devices and available services for potential access points.