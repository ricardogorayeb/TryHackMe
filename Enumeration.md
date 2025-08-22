
## Commands to achieve the flags on TryHackMe - Enumeration.

*Note: I’ll only provide the commands needed to achieve the flags or obtain the results. I won’t share the actual flags—except for those that return a direct response. Tasks that were answered directly in the provided text will not be included here.*

**Task 3**
**What is the name of the Linux distribution used in the VM?**

```cat /etc/os-release```

**What is its version number?**

```cat /etc/os-release```

**What is the name of the other user who last logged in to the system?**

```last```

**What is the highest listening TCP port number?**

```ss -natupe ```

**What is the program name of the service listening on it?**

```sudo lsof -i :6667```

**There is a script running in the background. Its name starts with THM. What is the name of the script?**

```ps aux | grep -i thm```

**Task 4**
**What is the full OS Name?**

```systeminfo```

**What is the OS Version?**

```systeminfo```

**How many hotfixes are installed on this MS Windows Server?**

```systeminfo```

**What is the lowest TCP port number listening on the system?**

```netstat```

**What is the name of the program listening on that port?**

```ssh.exe```

**Task 5**
**Knowing that the domain name on the MS Windows Server of IP 10.201.39.94 is redteam.thm, use dig to carry out a domain transfer. What is the flag that you get in the records?**
```dig -t AXFR redteam.thm @10.201.39.94```
*This command have to be used on Attack Box*

**What is the name of the share available over SMB protocol and starts with THM?**
```net share```

**Knowing that the community string used by the SNMP service is public, use snmpcheck to collect information about the MS Windows Server of IP 10.201.39.94. What is the location specified?**

```./snmpcheck-1.9.rb -c public 10.201.39.94```
*This command have to be used on Attack Box and you need to install snmpcheck according to instructions on text.*