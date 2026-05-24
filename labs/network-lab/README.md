# Network Lab: Home Infrastructure & Security Experiments

I built this lab to bridge the gap between cybersecurity theory and real-world practice. Theory is essential, but there is no substitute for building, breaking, and fixing systems in a controlled environment.

This repository documents my home lab setup, the challenges encountered, and experiments conducted to understand network architecture and security dynamics.

---

## 🏗️ Lab Architecture

My lab is currently hosted on a local machine using **Oracle VM VirtualBox**.

* **Host Machine:** [Your OS/Hardware]
* **Attacker Machine:** Kali Linux (For scanning, enumeration, and payload testing)
* **Target Machine:** Ubuntu Server 22.04 LTS (Sandbox for services and vulnerability testing)
* **Networking:** Host-Only Adapter (Isolated environment to prevent traffic leakage)



---

## 🎯 Objectives
I’m moving away from following tutorials blindly. My goal is to understand the "why" behind the tools by focusing on:

- **Traffic Flow:** How data moves between the attacker and the target.
- **Firewall/IDS Behavior:** How security controls react to specific packets.
- **Problem Solving:** Learning to troubleshoot when things break (which happens often!).

---

## 🛠️ Troubleshooting Log
I believe the best way to learn is by failing. This is a running log of the hurdles I’ve hit and how I resolved them.

### 1. Kali Linux VM "File Not Found" (Import Error)
* **Context:** Trying to "New" a VM using a pre-built appliance image.
* **Fix:** Instead of creating a new VM, used `Machine` > `Add` and selected the `.vbox` file directly.
* **Takeaway:** Always identify if you are using an `.iso` (manual install) or an `.vbox` (pre-built appliance).

### 2. Inter-VM Communication
* **Context:** VMs could not `ping` each other.
* **Fix:** Changed network adapter settings from `NAT` to `Host-Only`.
* **Takeaway:** Understanding network modes is non-negotiable for lab isolation.

### 3. Netplan Syntax Errors (Ubuntu)
* **Context:** Static IP configuration in `/etc/netplan/` kept failing.
* **Fix:** YAML is strict—I was using **tabs** instead of **spaces**. Fixed the indentation to use two spaces.
* **Verification:**
    ```bash
    sudo netplan apply
    ip a
    ```
* **Takeaway:** Precision matters. Invisible typos (like tabs vs. spaces) can break system configs.

---

## 🚀 Active Projects
* **[Reverse TCP Payloads]:** Testing how shells behave through internal networks.
* **[Network Traffic Analysis]:** Using Wireshark to monitor packet interactions.

---

*If you are looking at this and notice a configuration flaw, I would love the feedback. I am here to learn.*
