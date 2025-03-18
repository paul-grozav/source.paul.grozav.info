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
PS C:\> Install-Package Docker -ProviderName DockerMsftProvider -Force

# List installed packages
PS C:\> Get-Package



# ==== Services ====
# Create service (you can see it later in services GUI)
CMD C:\> C:\windows\system32\sc.exe create paul_app1 binpath= "\"C:\Program Files\info.grozav.paul.app1/app.exe\" --parameter-one \"parameter value\"" DisplayName= "Paul Grozav App1" start= auto

# Delete service (might have to restart -not just refresh- the services GUI)
CMD C:\> C:\windows\system32\sc.exe delete paul_app1

# Start and stop services
CMD C:\> net start paul_app1
CMD C:\> net stop paul_app1



# ==== Simple operations with files ====
# Remove a directory recursively
CMD C:\> rmdir /s dir_name
# Remove a directory recursively - force(do not ask)
CMD C:\> rmdir /s /q dir_name

# Rename file (or folder)
CMD C:\> move file_name new_file_name

# Create file with content
CMD C:\> echo my content > my_file.txt

# if condition
CMD C:\> if 1==1 (echo true) else (echo false)
CMD C:\> if "foo"=="bar" (echo true) else (echo false)
CMD C:\> if exists C:\some_file.txt (echo true) else (echo false)
CMD C:\> if not exists C:\some_file.txt (echo true) else (echo false)
CMD C:\> (if 1==1 (echo true) else (echo false)) && echo "either_way"

# chain commands
# Run cmd1 and then cmd2
CMD C:\> cmd1 & cmd2
# Run cmd1 and if it is successful then run cmd2
CMD C:\> cmd1 && cmd2
# Run cmd1 and if it fails then run cmd2
CMD C:\> cmd1 || cmd2

# print content of file
CMD C:\> more a.txt

# list folder contents
CMD C:\> dir

# copy file
CMD C:\> copy a.txt b.txt

# Comments
CMD C:\> :: this line is a comment
CMD C:\> rem this line is a comment
CMD C:\> dir & :: this is an inline comment (cmd dir is ran)

# Setting variables
CMD C:\>set a=5 
CMD C:\>set a=foo bar

# Printing variable values
CMD C:\>echo %a% 
foo bar
```

## 3. Batch script sample (CMD)
```bash
C:\>more a.bat
:: ========================================================================= ::
:: Author: Tancredi-Paul Grozav <paul@grozav.info>
:: ========================================================================= ::
:: Turn off printing commands before running them
@echo off

:: Call function 1
echo calling_function_1
call :function_1

:: Call function 2
echo calling_function_2
call :function_2

echo End of script.
:: this instruction exists the subroutine or script with exit code 0
goto :eof
:: This instruction exists the script with the given exit code
:: exit /b 59
:: You can then print the exit code using:
:: echo Exit Code is %errorlevel%
:: ========================================================================= ::
:function_1
  echo function 1
  goto :eof
:: ========================================================================= ::
:function_2
  echo function 2
  goto :eof
:: ========================================================================= ::

C:\>call a.bat 
calling_function_1 
function 1
calling_function_2
function 2 
End of script.

C:\>
```

## 4. Windows 11 install
Get .iso from Microsoft's website. Note that writing it to a USB stick with `dd`
is not ok, because the filesystem is copied as an ISO filesystem, and not as a
FAT32 filesystem, that is required for UEFI boot. This can mislead you to think
that the installer OS is missing a hardware driver, when in fact the installer
OS was not loaded properly. The message it prints is:
```txt
Windows 11 Setup
Install driver to show hardware
Please select the driver you want to install to make your hardware discoverable

Browse folder with drivers       Browse

Driver description              |  Driver file path

[x] Hide drivers that aren't compatible with this computer's hardware

A media driver your computer needs is missing. This could be a DVD, USB or Hard disk driver. If you have a CD, DVD, or USB flash drive with the driver on it, please insert it now.

Microsoft   Support   Legal    Back   Install
```

So, instead, an easy solution to make the USB bootable, is to use
[Ventoy](https://github.com/ventoy/Ventoy/releases):
```sh
wget https://github.com/ventoy/Ventoy/releases/latest/download/ventoy-1.0.xx-linux.tar.gz
tar -xf ventoy-1.0.xx-linux.tar.gz
cd ventoy-1.0.xx
sudo ./Ventoy2Disk.sh -i /dev/sdX
```
This will create 2 partitions, one for Ventoy itself, and another one that is
empty. You can place your .iso files there and when you boot from the USB stick,
the Ventoy software will display a boot menu with your .iso files as options.
So simply copy the Windows 11 iso to that partition, and select it in the boot
menu. This will start the installer properly.

Of course, if you are running on a Windows operating system, you can use the
[media creation tool](
  https://www.microsoft.com/en-us/software-download/windows11).

You can use `Shift + F10` during the installer, to open a Command Prompt and
check the devices and drivers, using command:
`pnputil /enum-devices /connected` which will list the devices and the driver
if loaded.

If you don't want to connect the computer to the internet, or don't want to
login with your (/ create a) Microsoft account, then you can open a cmd prompt
(`Shift + F10`) and type `OOBE\BYPASSNRO`, once you press enter, the computer
will reboot and you will have an "I don't have internet" option that you can
click, and create a local account. 
