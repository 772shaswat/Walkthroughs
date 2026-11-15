# 0x41haz Walkthrough

## Tools Used
- file
- binwalk
- strings
- Ghidra

## Step 1: Initial Analysis
- Download the executable file.
- Gather initial information:
  - file <binary>
  - binwalk <binary>
  - strings <binary>
- file helps identify the file type and architecture.
- binwalk checks for embedded or hidden binary data.
- strings extracts human-readable strings from the executable.

## Step 2: Reversing the Binary
- Open the executable in Ghidra.
- Allow auto-analysis to complete.
- Navigate to the entry function.

### Decompiled Entry Function

```c
void processEntry entry(undefined8 param_1,undefined8 param_2)

{
 undefined1 auStack_8 [8];

 __libc_start_main(FUN_00101165,param_2,&stack0x00000008,FUN_00101240,FUN_001012a0,param_1,
                   auStack_8);
 do {
 } while( true );
}
```

- The entry function does not contain the actual challenge logic.
- It calls several functions, with the main logic likely stored inside one of them.

## Step 3: Locating the Main Function
- Since this is an easy challenge, examined the first referenced function:
  - FUN_00101165
- This function contains the password validation logic.

### Decompiled Function

```c
undefined8 FUN_00101165(void)

{
 size_t sVar1;
 char local_48 [42];
 char local_1e [14];
 int local_10;
 int local_c;

 builtin_strncpy(local_1e,"2@@25$gfsT&@L",0xe);
 puts("=======================\nHey , Can You Crackme ?\n=======================");
 puts("It's jus a simple binary \n");
 puts("Tell Me the Password :");
 gets(local_48);
 sVar1 = strlen(local_48);
 local_10 = (int)sVar1;
 if ((int)sVar1 != 0xd) {
   puts("Is it correct , I don't think so.");
   exit(0);
 }
 local_c = 0;
 while( true ) {
   if (0xc < local_c) {
     puts("Well Done !!");
     return 0;
   }
   if (local_1e[local_c] != local_48[local_c]) break;
   local_c = local_c + 1;
 }
 puts("Nope");
 exit(0);
}
```

## Step 4: Recovering the Password
- The function contains a hardcoded string:

  - 2@@25$gfsT&@L

- The program compares user input against this value.
- Copy the string and provide it as input when executing the file.

## Step 5: Retrieving the Flag
- Execute the binary.
- Enter:

  - 2@@25$gfsT&@L

- The program returns:

  - Well Done !!

- The flag is:

  - THM{2@@25$gfsT&@L}

## Summary
- Used file, binwalk, and strings for initial reconnaissance.
- Opened the binary in Ghidra.
- Located the real program logic in FUN_00101165.
- Identified the hardcoded password used for validation.
- Entered the password and retrieved the flag.
