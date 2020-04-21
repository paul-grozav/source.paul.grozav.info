---
layout: page
ptitle: Windows maintenance
---

## 1. Speed up windows
====
1. Boot time scan
2. AntiVirus full update
3. Full system deep scan
4. Uninstall all programs
5. Cleanup disk tool
6. Defragment disks
7. Boot time chkdsk checking for errors and bad blocks
8. System error logs

## 2. PowerShell management
`CMD C:\>` commands are meant to be run in Command Prompt(cmd.exe), while the `PS C:\>` commands are meant to be run in PowerShell.
```bat
# Check if you can connect to a `host:port` combination:
PS C:\> Test-NetConnection -ComputerName "win.server.paul.grozav.info" -Port 8173 -InformationLevel "Detailed"
ComputerName            : win.server.paul.grozav.info
RemoteAddress           : 192.168.0.17
RemotePort              : 8173
NameResolutionResults   : 192.168.0.17
MatchingIPsecRules      :
NetworkIsolationContext :
InterfaceAlias          : vEthernet (Ethernet) 14
SourceAddress           : 172.28.222.8
NetRoute (NextHop)      : 172.28.208.1
TcpTestSucceeded        : True
PS C:\>

# Install package
Install-Package Docker -ProviderName DockerMsftProvider -Force

# List installed packages
Get-Package

# ==== Services ====
# Create service (you can see it later in services GUI)
CMD C:\> C:\windows\system32\sc.exe create paul_app1 binpath= "\"C:\Program Files\info.grozav.paul.app1/app.exe\" --parameter-one \"parameter value\"" DisplayName= "Paul Grozav App1" start= auto

# Delete service (might have to restart -not just refresh- the services GUI)
CMD C:\> C:\windows\system32\sc.exe delete paul_app1

# Start and stop services
CMD C:\> net start paul_app1
CMD C:\> net stop paul_app1
```
