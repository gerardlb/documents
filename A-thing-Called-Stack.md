
```ASM
foo:
    pushl %ebp
    mov %esp, %ebp
    pushl %edi
    pushl %esi
    pushl %ebx
    sub $0x90, %esp
    movl $0x1, (%esp)
    movl $0x2, 0x4(%esp)
    movl $0x3, 0x8(%esp)
    movl $0x4, 0xc(%esp)

```



Initial esp 	
esp -4 		{[push edbp]}
esp -4 		[mov %esp, %ebp]
esp -8		[pushl %edi]
esp -12 	[pushl %esi]
esp -16		[pushl %ebx]
esp -160 	[sub $0x90, %esp; move de esp pointer by 144 bytes to allocate memory]
esp -160 	[movl $0x1, (%esp); assign value 1 to this esp position (esp-160)]
esp -160 	[movl $0x2, 0x4(%esp); Assign value 2 to the (esp -160+4) position, but esp addres is maitained]
esp -160	[movl $0x3, 0x8(%esp) ;Assign value 3 to the (esp -160+8) position, but esp addres is maitained]
esp -160	[movl $0x4, 0xc(%esp) ; ;Assign value 4 to the (esp -160+12) position, but esp addres is maitained]	
	
So the diference in ESP is 160, in hex 0xa0	
