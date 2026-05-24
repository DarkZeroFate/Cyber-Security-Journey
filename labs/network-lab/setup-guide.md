# Setup Guide: Lab Infrastructure

This guide documents how I built my local network lab. I designed this setup to be isolated, safe for testing payloads, and easy to tear down or rebuild.

## Lab Environment
- **Hypervisor:** Oracle VM VirtualBox
- **Attacker Machine:** Kali Linux (202X.X)
- **Target Machine:** Ubuntu Server 22.04 LTS
- **Networking:** Host-Only Adapter (Isolated environment)

## Network Architecture
The goal of this lab is to create a controlled space. By using a **Host-Only Adapter**, I ensure that my testing traffic stays within the VM network and doesn't accidentally leak into my home WiFi or ISP network.

- **Host Machine:** My physical laptop.
- **Kali Linux IP:** `192.168.56.10`
- **Ubuntu Server IP:** `192.168.56.20`

## Step-by-Step Setup

### 1. Hypervisor Installation
I used Oracle VirtualBox because it's stable and widely documented. 
* *Note:* If you're running this on a Windows host, make sure "Virtualization Technology" is enabled in your BIOS/UEFI.

### 2. VM Configuration
- **Kali Linux:** Imported as a pre-built appliance. (Check out my `troubleshooting.md` if you have issues with VM file recognition).
- **Ubuntu Server:** Installed from the official ISO image. I kept the installation minimal (no GUI) to save resources and force myself to get comfortable with the Command Line Interface (CLI).

### 3. Networking & Static IP Configuration
This was the trickiest part. I had to configure static IPs so my machines could reliably talk to each other without using DHCP.

**Important Lesson:** I learned the hard way that YAML configuration files (used in Netplan) are extremely sensitive to indentation. Tabs will break your config. **Always use spaces.**

To apply my network changes, I used:
```bash
sudo netplan generate
sudo netplan apply

