# Room 404 Walkthrough

## Tools Used
- Nmap
- Gobuster
- Wget
- Git

## Step 1: Enumeration
- Added the machine's IP to /etc/hosts.
- Verified services with:
  - nmap room404
- Found two open ports:
  - 22 (SSH)
  - 8080 (HTTP)

## Step 2: Website Analysis
- The room description states that source code has been exposed and must be dumped.
- Accessed the website on port 8080.
- Observed broken links and no obvious flag locations.

## Step 3: Directory Enumeration
- Used Gobuster:
  - gobuster dir -u http://room404/ -w /usr/share/wordlists/dirbuster/directory-list-2-3-medium.txt -x txt,php,js,git
- Included the git extension to check for leaked repository data.
- Discovered an exposed .git directory.

## Step 4: Downloading the Repository
- Initial attempts to browse files inside .git manually were slow and did not reveal useful information.
- Downloaded the entire Git directory:
  - wget --mirror -Bv http://room404/.git
- This mirrored the exposed repository to the attacker machine.

## Step 5: Restoring the Source Code
- Learned that Git stores committed files inside .git/objects instead of visible directories.
- Restored the repository contents with:
  - git checkout .
- This restored all tracked files from the current commit into the working directory.

## Step 6: Retrieving the Flag
- Examined the restored files.
- Found the flag inside the README file.
- Retrieved the flag.

## Summary
- Enumerated the target and confirmed the web service on port 8080.
- Used Gobuster to discover an exposed .git directory.
- Downloaded the repository using wget.
- Restored committed files with git checkout .
- Retrieved the flag from the recovered README file.
