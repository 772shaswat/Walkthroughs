# Agent Sudo Walkthrough

## Tools Used
- Nmap  
- Burp Suite  
- Hydra  
- FTP  
- SSH  
- Stegseek  

## Step 1: Finding Open Ports
- nmap <target_ip> → Quick scan for faster results.  
- nmap -sV -p- <target_ip> → Full scan, checks versions and all 65535 ports.  

## Step 2: Redirection
- Found HTTP server from Nmap results.  
- Website message: employees must set user-agent as codename to access secret page.  
- Used Burp Suite:  
  - Proxy → Browser → Intercept ON.  
  - Sent request to Intruder.  
  - Modified user-agent with A–Z.  
  - Response for C had code 302 (redirection).  
- Learned:  
  1. Agent C’s name is Chris.  
  2. Chris’s password is weak.  

## Step 3: FTP
- Anonymous login failed.  
- Brute-forced with Hydra:  
  hydra -l chris -P /usr/share/wordlists/rockyou.txt <target_ip> ftp -V  
- Logged in as Chris.  
- Downloaded files.  
- Found .txt file → password hidden in an image.  
- Used Stegseek to extract.  

## Step 4: SSH
- Extracted Agent J’s name and password from Stegseek output.  
- Logged in via SSH: ssh j@<target_ip>  
- Checked privileges: sudo -l → (ALL, !root) /bin/bash  
- Meaning: Agent J can run commands as any user except root.  

## Step 5: Escalation
- Exploit found online for this CVE.  
- Command: sudo -u#-1 /bin/bash  
  - sudo runs command as another user.  
  - -u specifies user ID.  
  - #-1 interpreted as 4294967295 (32-bit unsigned wraparound).
  - The kernel or sudo library doesn't actually allow UID 4294967295 to exist as a real user.
  - This special value is treated as "no UID" or "invalid UID".
  - Many implementations map this back to UID 0 for compatibility reasons, ultimately spawning a root shell.
  - Kernel maps this UID to root (UID 0).  
- Result: root shell access.  

## Step 6: Flags
- With root shell, accessed both user.txt and root.txt.  
- Agent R’s name mentioned in root.txt.  

## Summary
- Enumerated services with Nmap.  
- Discovered redirection via Burp Suite.  
- Brute-forced Chris’s FTP login.  
- Extracted Agent J’s credentials with Stegseek.  
- Exploited sudo misconfiguration with UID wraparound.  
- Escalated to root and retrieved both flags.  
