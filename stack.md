[https://medium.com/@Alibafaqas/elf-x86-stack-buffer-overflow-basic-1-writeup-rootme-686f63f86c34](https://)

[https://www.root-me.org/fr/Challenges/App-Systeme/ELF-x86-Stack-buffer-overflow-basic-1?action_solution=voir#ancre_solution](https://)


But that is alright. Remember the .passwd file in the current folder? Why don’t we just open it using Python too? Running the following command will do the work, and you will get the flag

```
(python -c 'print "1"*40 + "\xef\xbe\xad\xde"'; echo cat .passwd) | ./ch13
```




To understand why `echo cat .passwd` is used in the exploit, let's break down the components:

### 1. **Buffer Overflow Vulnerability**

- The program reads up to 45 bytes into a 40-byte buffer (`buf`), allowing an overflow.
- The goal is to overwrite the `check` variable (initialized to `0x04030201`) with `0xdeadbeef` to spawn a shell.

### 2. **Payload Construction**

- `python -c 'print "1"*40 + "\xef\xbe\xad\xde"'`:
  - Fills the 40-byte `buf` with `'1'` characters.
  - Overwrites `check` with `0xdeadbeef` (written in little-endian format: `\xef\xbe\xad\xde`).
- This payload ensures `check == 0xdeadbeef`, triggering the shell via `system("/bin/bash")`.

### 3. **Why `echo cat .passwd`?**

- After the Python command outputs the exploit payload, `echo cat .passwd` appends the command `cat .passwd` followed by a newline.
- When the exploited program spawns a shell via `system("/bin/bash")`, the shell inherits the **same stdin** (the pipe from the command).
- The shell immediately executes `cat .passwd` from the piped input, printing the contents of `.passwd` before exiting.

### 4. **Key Points**

- The pipe `|` feeds the combined output (payload + `cat .passwd`) into the program's stdin.
- The `fgets` call reads the first 45 bytes (the payload), leaving the remaining input (`cat .passwd\n`) for the spawned shell.
- This technique automates command execution in the exploited shell without manual interaction.

### Summary:

`echo cat .passwd` ensures the command to read the flag is passed directly to the shell spawned by the buffer overflow, automating the exploitation process.
