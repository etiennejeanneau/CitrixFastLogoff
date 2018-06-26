# CitrixFastLogoff
Reimplementing Fast Logoff from WEM or Ivanti using logoff script, Powershell and C#

Not tested in production!

Code:
```
C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe  -Command "& {$signature = 'const int CurrentSession = -1; static readonly IntPtr CurrentServer = IntPtr.Zero ; [DllImport(\"WFAPI.dll\", EntryPoint = \"WFDisconnectSession\")] private static extern bool WFDisconnectSession(IntPtr hServer, int iSessionId, int bwait); public static bool ICADisconnect(){ bool ok = WFDisconnectSession(CurrentServer, CurrentSession, 0); if (!ok) return false; return true;}'; Add-Type -MemberDefinition $signature -Name CustomName -Namespace CustomNamespace -PassThru; [CustomNamespace.CustomName]::ICADisconnect()}"
```

This script was created with the constraint of launching it from a command prompt. (cmd > powershell > C#)

Some explanations:
- this script is written in Powershell but calls a C#method in wfapi.dll (Citrix DLL)
- wfapi.dll seems to be a 32bit DLL only, that's why we call the 32bit version of powershell.
- I spent quite some time debugging issues with escaping characters!

The PowerShell and C# code was adapted from this blog post: https://smulpuru.wordpress.com/2014/03/02/disconnect-ica-session-from-inside/
