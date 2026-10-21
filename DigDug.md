# Dig Dug Walkthrough

## Tools Used
- Nmap
- Dig

## Step 1: Enumeration
- Added machine’s IP to /etc/hosts as server.
- Performed a scan for enumeration.
- Identified that the target is running a DNS service.

## Step 2: Understanding the Service
- The room description states that the DNS server responds to only one domain:
  - givemetheflag.com
- Since the target is a DNS server, the goal is to perform DNS lookups against it.

## Step 3: Finding a DNS Lookup Tool
- Searched for DNS-related tools:
  - apropos -a dns lookup
- Found multiple tools capable of performing DNS queries.
- Chose dig since it was referenced throughout the room and is commonly used for DNS enumeration.

## Step 4: Querying the DNS Server
- Sent a DNS request:
  - dig server givemetheflag.com
- The DNS response contained the flag.

## Summary
- Identified the target as a DNS server.
- Learned that it only responds to queries for givemetheflag.com.
- Used dig to perform a DNS lookup.
- Retrieved the flag directly from the DNS response.
