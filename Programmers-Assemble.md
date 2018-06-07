* Programmers Assemble

We get this ASM code:

```ASM
.global main

main:
    mov $XXXXXXX, %eax
    mov $0, %ebx
    mov $0x5, %ecx
loop:
    test %eax, %eax
    jz fin
    add %ecx, %ebx
    dec %eax
    jmp loop
fin:
    cmp $0x7ee0, %ebx
    je good
    mov $0, %eax
    jmp end
good:
    mov $1, %eax
end:
    ret
```

If we translate it to something easier to read:

```
main:
	eax=XXXXXXX
	ebx=0
	ecx=5

loop:
	if eax==0	jmp fin
  else
	ebx=ecx+ebx
	eax=eax-1
	jmp loop

fin:
	if ebx==0x7ee0==32480 jmp good
	else
	eax=0
	jmp end

good:
	eax=1

end:
		ret

```

In order to jump to "good" tag, we need that inside the "fin" tag ebx equals 0x7ee0 (32480 in decimal).
In the "loop" section, we are increasing in +5 the value of ebx while we decrease in -1 the value of eax. So we need to run the loop 32480/5=6496 times.
then the value of EAX must be 6496to Hex it's 0x1960. 
