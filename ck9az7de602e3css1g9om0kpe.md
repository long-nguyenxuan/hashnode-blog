## Map file-extension với application tương ứng

# Map extension với application tương ứng

Trong Windows, để xác định 1 file extension nào nên mở bằng application nào, phải qua 2 bước

1. Xác định extension đó thuộc loại file (`fileType`) gì bằng `assoc`
2. Xác định fileType đó nên mở bằng application nào bằng `ftype`

## associations with the "assoc" command

Cú pháp:

```cmd
assoc [.ext[=[fileType]]]
```

Ví dụ:

```cmd
assoc .log=txtfile
```

## Manage file type and program associations with the "ftype" command

Cú pháp:

```cmd
ftype [fileType[=[openCommandString]]]
```

ví dụ:

```cmd
ftype txtfile="D:\apps\npp\notepad++.exe" "%1"
```
