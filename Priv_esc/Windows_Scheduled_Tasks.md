# Windows Scheduled Tasks Abuse

## Identifying the task

1. use the following command to show scheduled tasks:
`schtasks /query /fo LIST /v`

2. We should seek interesting information after executing the above command:
- Author : task author
- TaskName : task display name
- Task To Run : what is the task and can we replace/modify it
- Run As User : will the task eventualy provide privilage access
- Next Run Time : needs to be running in the future so it gets executed

<img width="650" alt="image" src="https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/7b9d788a-114f-464a-8d20-320c7600f2eb">

3. use icacls agin to see the permisions:
`icacls C:\Users\steve\Pictures\BackendCacheCleanup.exe`

<img width="645" alt="image" src="https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/ac99a636-63dc-44ce-95ae-bab03b42698e">

4. create a reverse shell or use the `create_admin_user.cpp` script that can be found in the OSCP_Scripts folder
- use `x86_64-w64-mingw32-gcc create_admin_user.cpp -o <privUserCreate.exe>`
- or msfvenom payload
  
5. replace the old exe for the new payload you have created

6. wait for the scheduald task to execute and check if the user was created with `net user`

7. use Run-As to run powershell admin user

