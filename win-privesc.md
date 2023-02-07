# Windows Privilege Escalation Notes  

## Integrity levels  

Find group information and integrity level.  

`whoami /groups`  

Change admin password.  

`net user admin newpassw0rd!`  

To change to high integrity level while admin.  

`powershell.exe Start-Process cmd.exe -Verb runAs`  

## Application manifest  

The application manifest is an xml file that informs the os how to handle the program when it is started.  

Using SysinternalsSuite.  

`sigcheck.exe -a -m C:\Windows\System32\aBinary.exe`  

## Processes  

From SysinternalsSuite, use this binary to find out information about a running process.  

`procmon.exe`  

## Reg Keys  

Find attempts to interact with registry keys that give the error.  

`NAME NOT FOUND`  

To tamper with registry keys, we can find our hive with read/write access, this is current user HKEY_CURRENT_USER.  

`HKCU`  

