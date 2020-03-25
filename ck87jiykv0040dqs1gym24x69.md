## Git Submodule

# Git Submodule 

Trong một số tình huống, bạn cần 1 folder nào đó trong dự án của bạn chứa source code của 1 dự án khác, do 1 team khác phát triển, trong 1 repo khác, 
ví dụ, cái folder `other-team-app` kia là cái bạn cần nó phải "hiện diện" trong repo của mình, nhưng nếu đơn thuần là `copy&paste` cái folder đấy vào, thì khi team kia update code, thì bạn không biết để mà update theo 
trong tình huống này, bạn cần đến `git submodule`
```
\my-project
    \other-team-app
    \my-code-dir 
    \my-other-code-dir 
```
có vài #TIL ngắn cho vụ này như sau : 

## Bạn là PM (CM), và bạn cần add submodule từ 1 dự án khác 

```bash
cd /path/to/my-project

git submodule add ssh://git@mycompany.com/my-group/other-team-app.git 

# chú ý rằng khi bạn add submodule add bằng ssh thì người khác cũng sẽ phải pull bằng ssh,
# việc chọn https hay ssh thì tùy vào tình huống dự án của bạn 
```

lúc này khi check trong folder `/my-project/other-team-app` thay vì có  1 **folder** `[.git]` thì bạn sẽ có 1 _file_ `.git` chứa link tới cái revision tương ứng của repo tương ứng với submodule đó.  


## Bạn là member, và bạn clone 1 project có submodule 

```bash 
git clone --recursive [URL to Git repo]
```

lúc này đồng thời với việc pull project chính, thì git sẽ pull luôn bản latest của submodule về cho bạn 

## Bạn lỡ clone 1 cái project, nhưng không biết có submodule 

Có những lúc lười, cứ vào là `git clone ssh://..../my-project.git` rồi tính tiếp chứ trong đấy có submodule hay cái j thì ai mà để ý 😅.  
Trong tình huống này, bạn cần đến 

```bash
git submodule update --init --recursive 

# trong đó cái --recursive là xử lý trường hợp nested submodule 😒
# trong trường hợp mà dự án của bạn có tận hàng chục cái submodule thì có thể dùng: 
# git submodule update --init --recursive --jobs 8 
# để kéo 8 cái submodule song song 
```

## Bạn là members, `other-project` báo là mới có bản update

```bash 
# cd vào trong folder submodule 
cd other-team-app

# Checkout cái nhánh bạn cần 
git checkout master

# Update
git pull

# rồi cd ra project root của bạn 
cd ..

# giờ thì commit việc thay đổi này (bản chất là sẽ thay đổi chỗ cái file .git trong folder `/other-team-app` kia 
git commit -am "Pulled down update to other-team-app"
```
