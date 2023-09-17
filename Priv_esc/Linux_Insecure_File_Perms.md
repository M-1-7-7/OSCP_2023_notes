# Insecure File Permisions

## Cron Jobs

**Identifying Cron Jobs**

use this command to see cron jobs that have been run `grep "CRON" /var/log/syslog` or `cat /var/log/cron.log`
<img width="632" alt="image" src="https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/8b45af0f-06d9-49d4-96de-872180487943">

look at any of the commands, could we modify what is being executed to get priv shell?
<img width="621" alt="image" src="https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/5e4e36b1-42c5-4489-855f-9dba5cddd979">

if yes we should modify it will a reverse shell one liner to get a call back as root to our listener
<img width="628" alt="image" src="https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/173133ca-4c10-451c-b16a-f09eb605c0f5">

## Password Authentication Abuse

if we have write privilages to `/etc/passwd` we could injext a new root user
<img width="589" alt="image" src="https://github.com/M-1-7-7/OSCP_2023_notes/assets/108218328/7b3f7d77-f065-4ea6-9074-fbf6cde2add7">
