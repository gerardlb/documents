Objective get the admin access

Take note that the source the declaration of variables:

```C
    int accessLevel = 0xff;                           
    char username[16];                                
    char password[32];                                
    printf("Username (max 15 characters): ");         
    gets(username);                                   
    printf("Password (max 31 characters): ");         
    gets(password);                                   
```

it seems that there's no check in the length of the username, so it can be used to overflow.

And also Note the value you need to to get a value for "access": 

```C
    ...
    } else if (access < 0x30) {
        printf("Admin access granted!\n");
        printf("The flag is in \"flag.txt\".\n");
        system("/bin/sh");
      }
      ...  
```

To gain the full access you have to overflow the username with 17 characters. The last one of the string must be a hex value less than 0x30.
One example of this could be: 
```
QWERTYUIOPASDFGH+
```

