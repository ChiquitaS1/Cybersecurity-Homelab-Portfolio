## Lab 1: Failed Login Investigation

## Objective

The Failed Login project aimed to establish a controlled environment to detect and investigate brute force using windows logs. The primary focus was to create failed login atempts to analyze log activity within Windows Even Viewers, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network log analysis, identifying suspicious activity, and basic threat detention.

## Skills Learned

  - Understanding of brute force attacks
  - Analyzing and interpreting network logs.
  - Ability to generate and recognize attack signatures and patterns.
  - Enhanced knowledge of network protocols and security vulnerabilities.

## Tools/Commands Used

  - Install wordlists
  - Ipconfig
  - nmap (port scans)
  - hydra -l
  - netsh advfirewall 

* Lab Setup:
  Attacker: Kali
  Target: Windows 10
  Virtualization: VirtualBox
  Nextwork: Host Only
## Steps
1. Enabled Logging 
2. Event Viewer -> Window Logs -> Security 
3. Simualte attack 

Ref 1: Kali Results 

RDP (Remote Desktop Protoco) is reachable 
hyrda connects to Windows 
username/password correct
User is not allowed to login through Remote Destop 
<img width="848" height="421" alt="Kali Hydra Results Ping but user not allowed to log in" src="https://github.com/user-attachments/assets/52cf8f6a-a792-4161-aa9c-c64e1dcb7963" />

Ref 2: Windows Event Viewer Logs 

Successful factors:
1. Logon Type 2 (Network) - confirms this is a network logon and not manaual typing at the screen
2. Timing anf Event Count
3. Identifying the attack
       - Source Network Address
       - Account Name 
<img width="641" height="632" alt="Windows Security Log Results" src="https://github.com/user-attachments/assets/8c9ebc5c-da3a-4658-a375-d8b385af2dfb" />

Ref 3: Windows Firewall 

Enabling firewall and having it block the attacking system (Kali)
<img width="935" height="317" alt="Windows FirewallRule" src="https://github.com/user-attachments/assets/f39a4e4d-9fb5-443c-ac39-4f7f938b6dbe" />
<img width="798" height="215" alt="Windows10 nestsh Results" src="https://github.com/user-attachments/assets/289f8ce0-3f6a-4296-89e3-e3dff0e27a1e" />
