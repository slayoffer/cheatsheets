In PowerShell, aliases are not stored in a specific file by default. Instead, they are loaded into the current session when you create them using the `New-Alias` or `Set-Alias` cmdlets. These aliases will be lost when you close the session.

If you want to create persistent aliases that are available every time you open a new PowerShell session, you can add the alias commands to your PowerShell profile script. The profile script is a PowerShell script that runs when you start a new PowerShell session. To locate your profile script, follow these steps:

1. Open a PowerShell window.
2. Type `$PROFILE` and press Enter. The output will show the path to your PowerShell profile script. The path usually looks like this:

```
C:\Users\YourUserName\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

If the file does not exist, you can create it using the following command:

```
New-Item -Type File -Path $PROFILE -Force
```

Now, open the `Microsoft.PowerShell_profile.ps1` file in a text editor (like Notepad or Visual Studio Code) and add your alias commands. For example:

```
New-Alias -Name np -Value "notepad.exe"
```

Save the file and close the text editor. The next time you open a new PowerShell session, your custom aliases will be available.