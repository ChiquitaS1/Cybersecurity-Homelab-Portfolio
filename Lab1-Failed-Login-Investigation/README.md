## Lab 1: Failed Login Investigation

## Objective

The Failed Login project aimed to establish a controlled environment to detect and investigate brute force using windows logs. The primary focus was to create failed login atempts to analyze log activity within Windows Event Viewers, generating test logging to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network log analysis, identifying suspicious activity, and basic threat detention.

## Skills Learned

  - Understanding of brute force attacks
  - Analyzing and interpreting network logs.
  - Ability to generate and recognize attack signatures and patterns.
  - Enhanced knowledge of network protocols and security vulnerabilities.

## Skills Demonstrated
 - Log analysis
 - Windows Event Investigation
 - Network scanning
 - Brute force detection
 - Incident response
 - SIEM fundamentals
 - Endpoint monitoring
 - Virtualization
 - Linux command line

# Tools & Technologies 

| Tool | Purpose |
|------|---------|
|Kali Linux | Attacker Machine |
| Windows 10 | Target Machine |
| Hydra | Brute force simulation |
|Sysmon| Endpoint logging |
| VirtualBox | Virtualization |

---  
## Network Topology

```text
Kali Linux VM ---> Windows 10 VM
      |                |
   Hydra Attack     Sysmon Logs
                       |
                    Event Viewer
```
# Lab Setup 

## Step 1: Configure Window Target 
```powershell
net user tester Password123 /add
```

Enable RDP:
- Settings → Remote Desktop → Enable

---

## Step 2: Install Sysmon

```powershell
Sysmon64.exe -i sysmonconfig.xml
```

Verify installation:
```powershell
Get-Service Sysmon64
```

---

## Step 3: Verify Connectivity

From Kali:

```bash
ping 192.168.XX.XXX
```

Scan ports:

```bash
nmap 192.168.XX.XXX
```

---

# Attack Simulation

## Hydra Brute Force Attack

Command used:

```bash
hydra -l testuser -P /usr/share/wordlists/rockyou.txt rdp://192.168.XX.XXX
```
## Expected Outcome 
- Multiple failed login attempts
- Security Event Logs generated

---
# Detection & Investigation

## Windows Event Viewer

Location:
```Event Viewer
Windows Logs → Security
```

Relevant Event IDs:

| Event ID | Description |
|----------|-------------|
| 4625 | Failed login |
| 4624 | Successful login |

---
# Incident Response Actions

## Disable Compromised User

```powershell
net user testuser /active:no
```

## Block Attacker IP

```powershell
netsh advfirewall firewall add rule name="Block Attacker IP" dir=in action=block remoteip=192.168.XX.XXX
```
---
# Evidence & Screenshots

## Hydra Output
✅ RDP (Remote Desktop Protoco) is reachable 

✅ hyrda connects to Windows

✅ username/password correct

❌ User is not allowed to login through Remote Destop 

<img width="848" height="421" alt="Kali Hydra Results Ping but user not allowed to log in" src="https://github.com/user-attachments/assets/52cf8f6a-a792-4161-aa9c-c64e1dcb7963" />

## Windows Event Viewer Logs 

Successful factors:
1. Logon Type 2 (Network) - confirms this is a network logon and not manaual typing at the screen
2. Timing anf Event Count
3. Identifying the attack
       - Source Network Address
       - Account Name 
<img width="641" height="632" alt="Windows Security Log Results" src="https://github.com/user-attachments/assets/8c9ebc5c-da3a-4658-a375-d8b385af2dfb" />

## Windows Firewall 

Enabling firewall allowing Windows to block the attack
<img width="935" height="317" alt="Windows FirewallRule" src="https://github.com/user-attachments/assets/f39a4e4d-9fb5-443c-ac39-4f7f938b6dbe" />
<img width="798" height="215" alt="Windows10 nestsh Results" src="https://github.com/user-attachments/assets/289f8ce0-3f6a-4296-89e3-e3dff0e27a1e" />

---

# Challenges & Troubleshooting

| Issue | Resolution |
|-------|-------------|
| Hydra failed to connect | Verified RDP was enabled |
| Mouse not working in VM | Installed VirtualBox Guest Additions |
| Firewall command failed | Used elevated PowerShell |

# Lessons Learned

- Learned how failed login attempts appear in Windows logs
- Understood how brute force attacks generate SIEM alerts
- Practiced basic incident response procedures
- Improved troubleshooting skills with networking and virtualization

