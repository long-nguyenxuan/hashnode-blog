## Visual Studio Test Project "need migration" error

bằng một cách thần kì nào đó thì để mở Test Project trong vs2019 cần phải enable cái **extension** là Developer Analytics Tools, 
kết quả là khi disable extension này đi thì không tạo được Test Project. 

https://developercommunity.visualstudio.com/content/problem/945957/unit-test-projects-depending-on-developer-analytic.html

Lúc này nếu cố tình tạo Test Project thì sẽ bị lỗi sau 

```
---------------------------
Microsoft Visual Studio
---------------------------
The project file 'C:\Users\longnx\AppData\Local\Temp\gygo1rlt.cwq\Temp\IbsEntryToolTests02.csproj' cannot be opened.
There is a missing project subtype.
Subtype: '{3AC096D0-A1C2-E12C-1390-A8335801FDAB}' is unsupported by this installation.
---------------------------
OK   Help   
---------------------------
```
