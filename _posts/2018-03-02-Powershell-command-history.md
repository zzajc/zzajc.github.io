---
layout: post
title: "Powershell command persistent history"
date: 2018-03-02
---
I Always forget commands that I write to Powershell once upon a time.

In linux this functionality is present out-of-box which is very nice a one does not understand why this is not the same with Powershell.
After searching internet I found the solution burried withing many not working and incomplete. Following are the steps for Windows 8.1

First you need to create a profile file if not already existing. For me the path for the file was:

```
C:\Users\Jiri\Documents\WindowsPowershell\Microsoft.Powershell_profile.ps1
```

The code that I needed can be found on 
[stackoverflow](https://stackoverflow.com/a/23932713/2128557)

To remove duplicates i tweaked the previous code by suggestion from
[another stackoverflow](https://stackoverflow.com/a/1439174/2128557)

To enable arrows to work with history a plugin is needed for Powershell version 4 and below. Windows 10 already contains this plugin.
[PSReadLine](https://github.com/lzybkr/PSReadLine)

Before installing the PSReadLine plugin a package manager needs to be installed from
[Microsoft download](https://www.microsoft.com/en-us/download/details.aspx?id=49186)

Finally install the plugin from Powershell with admin rights by

```
Install-Module PSReadLine
```


After merging all sources the final profile file contents are as following:

```
######### Persistent History ############
# Save last 200 history items on exit
$MaximumHistoryCount = 2000
$historyPath = Join-Path (split-path $profile) history.clixml

# This module adds support for up/down arrow for browsing history and other usefull stuff
Import-Module PSReadLine

# Hook Powershell's exiting event & hide the registration with -supportevent (from nivot.org) & remove duplicates (from Keith Hill)
Register-EngineEvent -SourceIdentifier powershell.exiting -SupportEvent -Action {
    Get-History -Count $MaximumHistoryCount | Group CommandLine | Foreach {$_.Group[0]} | Export-Clixml $historyPath
}.GetNewClosure()

# Load previous history, if it exists
if ((Test-Path $historyPath)) {
    Import-Clixml $historyPath | ? {$count++;$true} | Add-History
    Write-Host -Fore Green "`nLoaded $count history item(s).`n"
}

# Aliases and functions to make it useful
New-Alias -Name i -Value Invoke-History -Description "Invoke history alias"
Rename-Item Alias:\h original_h -Force
function h { Get-History -c  $MaximumHistoryCount }
function hg($arg) { Get-History -c $MaximumHistoryCount | out-string -stream | select-string $arg }
```

