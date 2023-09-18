# Comands to execute  to get flags for labs

### Chapter 16 - Windows Privilege Escalation

**16.1.2**
Q1) Check the users of the local group Remote Management Users on CLIENTWK220 (VM #1). Enter a user which is in this group apart from steve.
  - Use VM #1
  - netcat to machnine `nc <machine ip> 4444`
  - use this command `Get-LocalGroupMember "Remote Management Users"` and the flag should be the username `daveadmin`

Q2) Enumerate the installed applications on CLIENTWK220 (VM #1) and find the flag.
  - Use VM #1
  - netcat to machnine `nc <machine ip> 4444`
  - use this command `Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*"` and the flag will be in the output

Q3) We'll now use an additional machine, CLIENTWK221 (VM #2), to practice what we learned in this section. Access the machine via RDP as user mac with the password IAmTheGOATSysAdmin!. Identify another member of the local Administrators group apart from offsec and Administrator.
  - Use VM #1
  - xfreerdp to machnine `xfreerdp /u:mac /p:IAmTheGOATSysAdmin! /v:192.168.190.221 /h:650`
  - Start powershell
  - use this command `Get-LocalGroupMember "Administrators"` and the flag should be the username `roy`

Q4) Enumerate the currently running processes on CLIENTWK221 (VM #2). Find a non-standard process and locate the flag in the directory of the corresponding binary file.
  - Use VM #1
  - xfreerdp to machnine `xfreerdp /u:mac /p:IAmTheGOATSysAdmin! /v:192.168.190.221 /h:650`
  - Start powershell
  - use this command to find the proccess name `Get-Process`
  - once the name is identified use this comand to find the binary `Get-Process <proccess name> | Select-Object Path`
  - go to the directory where the exe is and `type flag.txt`

    

