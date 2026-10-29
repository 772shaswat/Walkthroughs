# Pickle Rick Walkthrough

## Tools Used
- Nmap
- Gobuster

## Step 1: Reconnaissance
- Added the target to /etc/hosts for convenience.
- Verified connectivity using ping.
- Performed scans:
  - nmap pickle
  - nmap -sV pickle
- Found two open ports:
  - 22 (SSH)
  - 80 (HTTP)

## Step 2: Website Enumeration
- Opened the website.
- Nothing immediately stood out.
- Viewed the page source.
- Found a comment containing a username.
- This username could potentially be used for SSH authentication or a web login page.

## Step 3: Directory Enumeration
- Used Gobuster:
  - gobuster dir -u http://pickle -w /usr/share/wordlists/dirbuster/directory-2-3-medium.txt -x txt,php,js,css,xml,html
- Adding multiple extensions helps prevent missing hidden files.

### Interesting Pages Found
- login.php
- portal.php
- robots.txt

## Step 4: Credentials Discovery
- Opened robots.txt.
- Found a string that appeared to be a password.
- login.php and portal.php contained the same login page.
- Used:
  - Username from page source.
  - Password from robots.txt.
- Successfully authenticated.

## Step 5: Command Execution
- After logging in, a command panel was available.
- Tested basic commands:
  - whoami
  - pwd
  - ls
- Found a file containing the first flag.
- cat was filtered, so alternative commands were used:
  - less
  - strings
- Retrieved the first flag.

## Step 6: Searching for the Second Flag
- Found clue.txt.
- Reading the file revealed:
  - "Look around the file system for the other ingredient."
- Also found a PHP file that displayed:
  - "Only the Real Rick can view this page."

## Step 7: User Enumeration
- Checked users with login shells:
  - less /etc/passwd | grep /bin/bash
- Enumerated home directories:
  - ls /home/
- Found:
  - rick
  - ubuntu

## Step 8: Second Flag
- Listed Rick's home directory:
  - ls /home/rick/
- Located the second ingredient.
- Used less to read its contents and retrieve the second flag.

## Step 9: Privilege Escalation
- Checked sudo privileges:
  - sudo -l
- Output:
  - (ALL) NOPASSWD: ALL
- The www-data user can execute any command as root without providing a password.

## Step 10: Final Flag
- Listed the root directory:
  - sudo ls /root/
- Located the final ingredient.
- Retrieved the third flag from the root directory.

## Summary
- Enumerated services with Nmap.
- Found a username in the website source.
- Discovered a password in robots.txt.
- Logged into the web command panel.
- Retrieved the first flag using alternative file-reading commands.
- Found the second flag in Rick's home directory.
- Exploited unrestricted sudo permissions.
- Retrieved the final flag from the root directory.
