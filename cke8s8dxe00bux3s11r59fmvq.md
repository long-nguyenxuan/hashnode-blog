## Update owner và permission của tất cả file trong 1 folder

Hôm nay có cái vấn đề nhỏ nhỏ, là khi mình chạy 1 phần mềm (game Dota2 😁) bằng 1 user mới (`MY_PC\User02`) thì có vài file, bằng cách nào đó dc set owner = `MY_PC\User01`, và permission chỉ có `MY_PC\User01` truy cập dc --> cho dù `MY_PC\User02` đã dc add vào trong nhóm `BUILTIN\Administrators` thì cũng ko ăn thua. 
Để xử lý thì rất đơn giản, cứ file nào không có permission thì chúng ta change owner sang `MY_PC\User01` rồi gán lại permission cho nó :D, cơ mà lặp đi lặp lại bộ thao tác right click chọn Properties mở bảng thuộc tính, change owner, lại mở properties, set permission cho vài chục nghìn file(23,134 files), thì rất dễ điên, nên mình viêt cái script này 

```powershell 
Get-ChildItem "E:\path\to\my\folder" -Recurse | 
ForEach-Object {
    $objFile = Get-Acl $_.FullName
    $objUser = New-Object System.Security.Principal.NTAccount(“MY_PC”, “User02”)
    $objFile.SetOwner($objUser)
    $acccesRule = New-Object System.Security.AccessControl.FileSystemAccessRule("BUILTIN\Administrators", "FullControl", "Allow")
    $objFile.SetAccessRule($acccesRule)
    Set-Acl -AclObject $objFile -path $_.FullName
}
```

