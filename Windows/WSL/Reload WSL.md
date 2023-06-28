Create this PowerShell script. I named it `killwsl.ps1` (note ps1 is a PowerShell script):

```
##@REM  Run via PowerShell +Admin

echo "listing all wsl tasks"
tasklist /M wsl*

echo "sending kill to wslservice"
taskkill /IM wslservice.exe /F

echo "Done.  Did your fans stop spinning?"
```

When (not IF) WSL hangs and is unresponsive, close your WSL terminals and run the `killwsl.ps1` script:

1. Open (or keep open) an Administrator PowerShell (via right-click "Run as Administrator")
2. Run `.\killwsl.ps1`

Open a PowerShell terminal as an administrator (Ctrl + Click if your terminal is already open), then run the following::

--> Note that it is important to run the command as an administrator, or else you will get an access denied error

`(Get-Process | Where-Object {$_.Name -eq "wslservice"}).Id | ForEach-Object {Stop-Process -Id $_ -Force}`

And then:

`Restart-Service WslService`

After that, you should be able to open a WSL tab from a non-admin terminal again.

or can make this more elegant with:

`(Get-Process | Where-Object {$_.Name -eq "wslservice"}).Id | ForEach-Object {Stop-Process -Id $_ -Force};Restart-Service -Name "wslservice"`

And even fancier with:

Write the code below in your PowerShell profile. If you don’t know where it is, run `echo $PROFILE`.  
If the file does not exists run `New-Item -ItemType File -Path $PROFILE -Force` to create.

Write the following at the end of the file:

```powershell
# WSL Reload
function Reboot-WSLService {
    $processId = (Get-Process | Where-Object {$_.Name -eq "wslservice"}).Id
    if ($processId) {
        Stop-Process -Id $processId -Force
    }
    Restart-Service -Name "wslservice"
}

New-Alias -Name reloadwsl -Value Reboot-WSLService

```

Close all terminal windows and reopen them.  
After that, whenever your WSL freezes, you can just run this command in an admin PowerShell terminal :  
`RebootWslService`

--> Admin still important to run the alias to avoid access denied error