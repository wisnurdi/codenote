##Menjalankan File Exe
Deklarasikan sebuah fungsi dengan sintaks di module sebagai berikut:
```VB6
  Declare Function ShellExecute Lib "Shell32.dll" Alias "ShellExecuteA" _
    (ByVal hwnd As Long, ByVal lpOperation As String, _
    ByVal lpFile As String, ByVal lpParameters As String, _
    ByVal lpDirectory As String, ByVal nShowCmd As Long) As Long
```
Kemudian panggil di form dengan contoh sintaks berikut:
```
Private Sub cmdMo21rt_Click()
    ShellExecute 1, "open", "mo21rt.exe", 0, 0, 1
End Sub
```
##Mengecek File dan Folder Eksis atau Tidak
Pertama, tambahkan references bernama Microsoft Scripting Runtime, biasanya fiolenya ada di C:\Windows\System32\scrrun.dll.
Selanjutnya, buat prosedur dengan contoh sebagai berikut
```
Sub
Dim fsysTemp As New FileSystemObject
Dim fileocx As String
fileocx = "C:\Program Files\Common Files\ESRI\Mo20.ocx"
If fsysTemp.FileExists(fileocx) Then
        ‘ada filenya
End If
End Sub
```
Untuk mengecek folder, fungsi FileExists(namafile) bisa diganti dengan FolderExists(namafolder).
##Menyalin File
```
  If Not fsysTemp.FileExists(“filetujuan.ext”) Then
    fsysTemp.CopyFile "filesumber.ext", “filetujuan.ext”, False
  End If
 ```
##Membuat File Text
Kita dapat menggunakan references yang sama, yakni Microsoft Scripting Runtime.
Sintaksnya sebagai berikut:
```
If Not fsysTemp.FileExists("C:\namafile.txt") Then
            streamLic = fsysTemp.CreateTextFile("C:\namafile.txt", False)
            streamLic.Write (“isi teksnya adalah ini”)
            streamLic.Close
        End If
```
##Mengecek Apakah Sudah Terinstal Dotnet
Buat sintaks di module sebagai berikut:
```
' Registry value type definitions
Public Const REG_NONE As Long = 0
Public Const REG_SZ As Long = 1
Public Const REG_EXPAND_SZ As Long = 2
Public Const REG_BINARY As Long = 3
Public Const REG_DWORD As Long = 4
Public Const REG_LINK As Long = 6
Public Const REG_MULTI_SZ As Long = 7
Public Const REG_RESOURCE_LIST As Long = 8

' Registry section definitions
Public Const HKEY_CLASSES_ROOT = &H80000000
Public Const HKEY_CURRENT_USER = &H80000001
Public Const HKEY_LOCAL_MACHINE = &H80000002
Public Const HKEY_USERS = &H80000003
Public Const HKEY_PERFORMANCE_DATA = &H80000004
Public Const HKEY_CURRENT_CONFIG = &H80000005
Public Const HKEY_DYN_DATA = &H80000006

' Codes returned by Reg API calls
Private Const ERROR_NONE = 0
Private Const ERROR_BADDB = 1
Private Const ERROR_BADKEY = 2
Private Const ERROR_CANTOPEN = 3
Private Const ERROR_CANTREAD = 4
Private Const ERROR_CANTWRITE = 5
Private Const ERROR_OUTOFMEMORY = 6
Private Const ERROR_INVALID_PARAMETER = 7
Private Const ERROR_ACCESS_DENIED = 8
Private Const ERROR_INVALID_PARAMETERS = 87
Private Const ERROR_NO_MORE_ITEMS = 259

' Registry API functions used in this module (there are more of them)
Private Declare Function RegOpenKey Lib "advapi32.dll" Alias "RegOpenKeyA" ( _
    ByVal hKey As Long, ByVal lpSubKey As String, phkResult As Long) As Long
Private Declare Function RegOpenKeyEx Lib "advapi32.dll" Alias "RegOpenKeyExA" ( _
    ByVal hKey As Long, ByVal lpSubKey As String, ByVal ulOptions As Long, _
    ByVal samDesired As Long, phkResult As Long) As Long
Private Declare Function RegQueryValueEx Lib "advapi32.dll" Alias "RegQueryValueExA" ( _
    ByVal hKey As Long, ByVal lpValueName As String, ByVal lpReserved As Long, _
    lpType As Long, ByVal lpData As String, lpcbData As Long) As Long
Private Declare Function RegEnumValue Lib "advapi32.dll" Alias "RegEnumValueA" ( _
    ByVal hKey As Long, ByVal dwIndex As Long, ByVal lpValueName As String, _
    lpcbValueName As Long, ByVal lpReserved As Long, lpType As Long, _
    ByVal lpData As String, lpcbData As Long) As Long
Private Declare Function RegCloseKey Lib "advapi32.dll" (ByVal hKey As Long) As Long
Private Declare Function RegCreateKey Lib "advapi32.dll" Alias "RegCreateKeyA" ( _
    ByVal hKey As Long, ByVal lpSubKey As String, phkResult As Long) As Long
Private Declare Function RegSetValueExString Lib "advapi32.dll" Alias "RegSetValueExA" ( _
    ByVal hKey As Long, ByVal lpValueName As String, ByVal Reserved As Long, _
    ByVal dwType As Long, ByVal lpValue As String, ByVal cbData As Long) As Long
Private Declare Function RegSetValueExLong Lib "advapi32.dll" Alias "RegSetValueExA" ( _
    ByVal hKey As Long, ByVal lpValueName As String, ByVal Reserved As Long, _
    ByVal dwType As Long, lpValue As Long, ByVal cbData As Long) As Long
Private Declare Function RegFlushKey Lib "advapi32.dll" (ByVal hKey As Long) As Long
Private Declare Function RegEnumKey Lib "advapi32.dll" Alias "RegEnumKeyA" ( _
    ByVal hKey As Long, ByVal dwIndex As Long, ByVal lpName As String, _
    ByVal cbName As Long) As Long
Private Declare Function RegDeleteKey Lib "advapi32.dll" Alias "RegDeleteKeyA" ( _
    ByVal hKey As Long, ByVal lpSubKey As String) As Long
Private Declare Function RegDeleteValue Lib "advapi32.dll" Alias "RegDeleteValueA" ( _
    ByVal hKey As Long, ByVal lpValueName As String) As Long

' This routine allows you to get values from anywhere in the Registry, it currently
' only handles string, double word and binary values. Binary values are returned as
' hex strings.
'
' Example
' Text1.Text = ReadRegistry(HKEY_LOCAL_MACHINE,
'   "SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon", "DefaultUserName")
'
Public Function ReadRegistry(ByVal Group As Long, ByVal Section As String, _
    ByVal Key As String) As String
Dim lResult As Long, lKeyValue As Long, lDataTypeValue As Long, lValueLength As Long, _
    sValue As String, td As Double
On Error Resume Next
lResult = RegOpenKey(Group, Section, lKeyValue)
sValue = Space$(2048)
lValueLength = Len(sValue)
lResult = RegQueryValueEx(lKeyValue, Key, 0&, lDataTypeValue, sValue, lValueLength)
If (lResult = 0) And (Err.Number = 0) Then
   If lDataTypeValue = REG_DWORD Then
      td = Asc(Mid$(sValue, 1, 1)) + &H100& * Asc(Mid$(sValue, 2, 1)) + _
          &H10000 * Asc(Mid$(sValue, 3, 1)) + &H1000000 * CDbl(Asc(Mid$(sValue, 4, 1)))
      sValue = Format$(td, "000")
   End If
   If lDataTypeValue = REG_BINARY Then
       ' Return a binary field as a hex string (2 chars per byte)
       TStr2 = ""
       For i = 1 To lValueLength
          TStr1 = Hex(Asc(Mid(sValue, i, 1)))
          If Len(TStr1) = 1 Then TStr1 = "0" & TStr1
          TStr2 = TStr2 + TStr1
       Next
       sValue = TStr2
   Else
      sValue = Left$(sValue, lValueLength - 1)
   End If
Else
   sValue = "Not Found"
End If
lResult = RegCloseKey(lKeyValue)
ReadRegistry = sValue
End Function
```
Selanjutnya, panggil fungsi untuk mengecek apakah sudah terinstal dotnet framework.
```
If ReadRegistry(HKEY_LOCAL_MACHINE, "SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Client\", "Install") = "001" Then
        chkDot.Value = vbChecked
Endif
```
##Membatalkan Form Unload
```
Private Sub Form_Unload(Cancel As Integer)
    Cancel = True
End Sub
```

##Menjalankan CommandPrompt
Contoh kali ini adalah meregister sebuah library pada sistem operasi.
```
Shell "Regsvr32.exe " & “file_library.dll”, vbHide
```

