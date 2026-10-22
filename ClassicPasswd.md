# Classic Passwd Walkthrough

## Tools Used
- Ghidra

## Step 1: Analyzing the Program
- Downloaded the executable provided in the room.
- Executed the program to understand its behavior.
- Program prompts for a username.
- Entering an incorrect username causes the program to terminate.
- Objective: Find the correct username to retrieve the flag.

## Step 2: Loading the Binary in Ghidra
- Imported the executable into Ghidra.
- Initial approach was to search for the string:
  - "Insert your username:"
- While browsing through the program, the string was located without needing a full search.

## Step 3: Finding the Verification Logic
- Right-clicked the string.
- Selected References To to find where it was used.
- Opened the referenced function in the decompiler.
- The decompiler revealed the program logic in C-like code.

## Step 4: Identifying the Correct Username
- Scrolled through the decompiled code.
- Found a TEST EAX,EAX check associated with string comparison.
- Observed that the user input was being compared against a hardcoded username.
- Copied the original value used in the comparison.

## Step 5: Retrieving the Flag
- Executed the program again.
- Entered the recovered username.
- The validation succeeded.
- The program displayed the flag.

## Summary
- Analyzed the binary using Ghidra.
- Located the username prompt string.
- Followed references to the validation function.
- Identified the hardcoded username through decompiled code analysis.
- Entered the correct username and retrieved the flag.
