# Windows Priv Esc (IF WE DONT HAVE WINPEAS/JAWS)

## Severl Key artifact to identify:

- Username and hostname:
    - `whoami`
- Group memberships of the current user
    - `whoami /groups`
- Existing users and groups
    - `Get-LocalUser`
    - `Get-LocalGroup`
    - `Get-LocalGroupMember <group name>`
- Operating system, version and architecture
    - `systeminfo`
- Network information
    - `ipconfig`
    - `route print`
    - `netstat -ano`
- Installed applications
    - `Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname`
    - `Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname`
- Running processes
    - `Get-Process`


## Artifacts to keep an eye out for:

**KeePass Password Manager:**
`Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue` to get all password manager databases stored in C:\

**XAMPP:**
`Get-ChildItem -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue` Searching for sensitive information in XAMPP directory

**Search for sesitive files and types:**
`Get-ChildItem -Path C:\Users\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx,*.ini -File -Recurse -ErrorAction SilentlyContinue` Searching for text files and password manager databases in the home directory of dave

**ASDF files:**
`cat Desktop\asdf.txt` is a comand line interface tool adn if you see `asdf.txt` we may find valuable information. 

**my.ini file:**
if we have the privilage we could see plain txt users and passwords with this command `type C:\xampp\mysql\bin\my.ini`

## Powershell Artifacts:

**Powershell History** 
- `Get-History` will get command history for current user
- `(Get-PSReadlineOption).HistorySavePath` will get the history path
- `type C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt` another history command to try 
- If you notice any txt files or credentials being enterd into locations/artifacts be sure to investigate.

  
