Network Lab: Home Infrastructure & Security Experiments
I built this lab because I wanted to stop just reading about cybersecurity and start doing it. Theory is important, but nothing beats actually breaking things and trying to fix them in a controlled environment.

This repository documents my home lab setup, the challenges I ran into, and the experiments I’m running to understand how networks and attacks actually work in the real world.



The Lab Setup
Currently, my lab is pretty lightweight and built on top of my host machine using Oracle VM VirtualBox.

Attacker Machine: Kali Linux (For scanning, enumeration, and testing payloads).

Target Machine: Ubuntu Server (My sandbox for testing services and vulnerabilities).

Networking: [Describe your setup here, e.g., Host-Only Adapter] to keep it isolated from my main host.

(Add a simple diagram or screenshot of your VirtualBox network settings here if you can)

Why I'm doing this
I’m tired of just following tutorials blindly. My goal with this lab is to understand the "why" behind the tools. Instead of just running nmap or metasploit commands, I want to see:

How traffic actually flows between two machines.

How firewalls react (or don't) to certain packets.

How to troubleshoot when things inevitably go wrong (which happens a lot!).



Lessons Learned & Troubleshooting
I believe the best way to learn is by failing. This is a running log of the issues I've hit while building this and how I solved them.

Issue: Kali Linux VM file not recognized.

Context: I downloaded the pre-built VM, but VirtualBox kept asking for an .iso file when I clicked "New."

Fix: I realized I was trying to install it from scratch instead of importing the existing VM. Using the "Add" (or "Machine" -> "Add") option with the .vbox file fixed it.

Takeaway: Always check if you're importing a pre-built appliance or setting up a new install from an ISO.




Technical Hurdle: Static IP Configuration
Challenge: Configuring a static IP address on my Ubuntu Server.

The Struggle:
I initially had a hard time setting up the network interface. Since I’m still getting used to nano (I accidentally messed up the controls a few times), I had trouble editing the Netplan configuration files.

The biggest hurdle was the YAML indentation. I didn't realize that YAML files are extremely strict about spacing—my configuration kept failing because I used tabs instead of spaces.

The Process (and how I fixed it):

The Tool: Learned the basics of nano (Ctrl+O to save, Ctrl+X to exit).

The Fix: Realized that netplan requires specific indentation. I rewrote the config, replaced tabs with spaces, and verified the structure.

The Test: Applied the changes using:
sudo netplan apply

Verification: Confirmed the IP was active using:
ip a

Key Takeaway:
Networking configurations are sensitive. Always double-check for syntax errors and whitespace issues before applying changes. Also, practicing nano shortcuts—even the simple ones—is a lifesaver in a CLI-only environment.




Current Projects
[Link-not-yet-configured]: Exploring Reverse TCP payloads.

[Link-not-yet-configured]: Basic network traffic analysis with Wireshark.

If you're reading this and notice something I've configured poorly, I'd honestly love the feedback. I'm here to learn.
