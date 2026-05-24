# Troubleshooting Log

This document is a living record of the hurdles I've faced while building my lab. Instead of just fixing them and moving on, I write them down so I (and hopefully others) don't make the same mistakes twice. 

*If you're stuck on a similar issue, I hope this helps.*

---

## 1. Kali Linux VM "File Not Found" (VirtualBox Import)
- **Problem:** When setting up my Kali VM, I couldn't get VirtualBox to recognize the downloaded file.
- **Context:** I kept clicking "New" in VirtualBox, which only searches for `.iso` files. The pre-built image I downloaded wasn't an ISO.
- **Solution:**
  1. I realized I didn't need to "Create a new VM."
  2. I used the `Machine` -> `Add` option in VirtualBox.
  3. I selected the `.vbox` file from the folder I extracted from the `.7z` archive.
- **Lesson Learned:** Always check the file type. ISOs are for manual installations; pre-built virtual appliances use project files like `.vbox`.

## 2. Inter-VM Communication (NAT vs. Host-Only)
- **Problem:** My Kali VM and Ubuntu Server couldn't communicate.
- **Context:** Both VMs were running, but I couldn't `ping` from one to the other.
- **Root Cause:** The network adapter was set to "NAT". This isolates each VM into its own private network behind the host machine.
- **Solution:** 1. I changed the network settings for both VMs to **Host-Only Adapter**.
  2. This forced them onto the same subnet (e.g., `192.168.56.x`), allowing them to talk to each other without touching my main network.
- **Lesson Learned:** Understanding network modes (NAT, Host-Only, Bridged) is non-negotiable before doing any traffic analysis or pentesting.

## 3. Netplan Syntax Errors (Ubuntu)
- **Problem:** My static IP configuration wouldn't apply, throwing cryptic errors.
- **Context:** Trying to edit the static IP in `/etc/netplan/01-netcfg.yaml`.
- **Solution:** I discovered that I was using **tabs** for indentation instead of **spaces**. YAML files are extremely strict about this. Once I replaced the tabs with two spaces, the command worked perfectly:
  ```bash
  sudo netplan apply
