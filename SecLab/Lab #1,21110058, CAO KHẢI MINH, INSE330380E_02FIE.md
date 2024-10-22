# Lab #1,21110058, CAO KHẢI MINH, INSE330380E_02FIE

# Task 1: **Software buffer overflow attack**
**Question 1**: Compile asm program and C program to executable code. 

\- Conduct the attack so that when C executable code runs, shellcode will be triggered and a new entry is  added to the /etc/hosts file on your linux. 

 You are free to choose Code Injection or Environment Variable approach to do. 

\- Write step-by-step explanation and clearly comment on instructions and screenshots that you have made to successfully accomplished the attack.

## **I. Set up the C program:**

### 1. Create a C file named `vuln.c`:
```sh
nano vuln.c
```
<img width="500" alt="Screenshot" src="https://github.com/CKMPeter/Information-Sec/blob/main/SecLab/nanoC.png?raw=true"><br>

After which use `crtl` +` o` to save ad `crtl`+`x` to exit.

### 3. Compile the newly written C file with `gcc`:

```sh
sudo gcc -m64 -fno-stack-protector -z execstack -o vuln vuln.c
```

`-m64`: Forces compilation in 64-bit mode (using libc6-dev-i386).

`-fno-stack-protector`: Disables the stack protection feature.

`-z execstack`: Marks the stack as executable.

</br>

## **II. Set up the NASM program:**

### 1. Create a NASM file named `attackCode.nasm`:

```sh
nano attackCode.nasm
```

<img width="500" alt="Screenshot" src="https://github.com/CKMPeter/Information-Sec/blob/main/SecLab/attackCode.png?raw=true"><br>

After which use `crtl` +` o` to save ad `crtl`+`x` to exit.

### 2. **Compile and Link the newly written NASM file**:

#### a. Compile the file:

```sh
nasm -f elf32 -o attackCode.o attackCode.nasm
```

**`-f elf32`**: This flag specifies the output format. In this case, it's generating a 32-bit **ELF**

**`-o attackCode.o`**: This specifies the output file's name. The resulting object file will be named `attackCode.o`

</br>

#### b. Link the file to an executable:

```sh
ld -m elf_i386 -o attackCode attackCode.o
```

</br>

#### c. Generate the `shellcode` from the executable:

```sh
objdump -d attackCode| grep '[0-9a-f]:' | grep -oP '\s\K[0-9a-f]+' | tr -d '\n' | sed 's/\(..\)/\\x\1/g'
```

**`objdump -d attackCode`**:

- Disassembles the binary file (`attackCode`) and shows its assembly instructions along with their corresponding machine code (opcode).

**`grep '[0-9a-f]:'`**:

- Filters out the lines from `objdump` output that contain opcodes (which are hexadecimal values) by looking for lines that have a hexadecimal number followed by a colon (`:`). These are typically the lines with the memory addresses and opcodes.

**`grep -oP '\s\K[0-9a-f]+'`**:

- This command extracts the hexadecimal opcodes from the filtered lines using a regular expression.

- ```sh
  \s\K[0-9a-f]+
  ```

  `\s`: Looks for a space before the opcodes.

  `\K`: Ignores everything before the opcodes (the memory addresses).

  `[0-9a-f]+`: Matches one or more hexadecimal characters (i.e., the machine code).

**`sed 's/\(..\)/\\x\1/g'`**:

- Uses `sed` (stream editor) to format the opcodes as shellcode by adding `\x` before each byte.
- `s/\(..\)/\\x\1/g`: Matches every two hexadecimal characters (which make up a byte) and adds `\x` before them.

</br>

#### d. Total summary of process:

<img alt="processNASM" src="https://github.com/CKMPeter/Information-Sec/blob/main/SecLab/attackCodeComplie.png?raw=true">

after running the `objdump` command you will get the shellcode to inject into the vuln program.

</br>

## III. EXECUTE THE ATTACK:











