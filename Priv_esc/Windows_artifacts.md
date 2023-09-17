# Windows Priv Esc (IF WE DONT HAVE WINPEAS/JAWS)

### Severl Key artifact to identify:

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


### Artifacts to keep an eye out for:

**KeePass Password Manager:**
`Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue` to get all password manager databases stored in C:\

**XAMPP:**
`Get-ChildItem -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue` Searching for sensitive information in XAMPP directory

**Search for sesitive files and types:**
`Get-ChildItem -Path C:\Users\dave\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue` Searching for text files and password manager databases in the home directory of dave

**ASDF files:**
`cat Desktop\asdf.txt` is a comand line interface tool adn if you see `asdf.txt` we may find valuable information. 

**my.ini file:**
if we have the privilage we could see plain txt users and passwords with this command `type C:\xampp\mysql\bin\my.ini`


