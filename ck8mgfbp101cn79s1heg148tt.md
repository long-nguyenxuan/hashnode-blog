## Hướng dẫn áp dụng Git flow trong dự án

## Vấn đề 

Khi làm dự án phần mềm, việc nhiều người cùng edit chung 1 file và sử dụng các tool version control để quản lý các phiên bản gần như là không thể tránh khỏi, trong quá trình làm việc như vậy, có nhiều best practice có thể áp dụng để tránh giẫm dân nhau, nổi tiếng nhất có thể kể đến git-flow của bác Vincent Driessen, mà sau đó Github và Gitlab chế lại của bác ấy thành Github Flow và GitLab Flow 

Bên dưới là 1 trong những cách như vậy, cái này lấy ý tưởng từ git-flow, và điều chỉnh một chút cho phù hợp với quy trình ở công ty mình đang làm việc (fsoft), gọi nó là git-fsoft-flow cũng được :P 

## Pre-condition

Đã có khái niệm cơ bản về version control và git 

## Target audience 

All member trong dự án dự định sử dụng Git, hoặc đang áp dụng Git và thấy nó "chẳng khác gì xx" 

## Core Idea

1. nhánh master chứa source production 
2. nhánh staging chứa source `đang-test` 
3. mỗi user có 1 nhánh "{username}" để push code hàng ngày. 
4. nhánh hotfix để xử lý bug production "thực-sự-hot"
5. user có thể tạo thêm các nhánh để fix bug staging, hoặc fix trong nhánh `{username}` miễn sao fix được bug và gửi vào trong staging được là OK. 

## Nguyên tắc 

1. **KHÔNG BAO GIỜ** `push` vào `master`  
  trong master chỉ chứa source được merge.  
  note nhỏ: để phòng tránh việc này, gitlab nó protect luôn cái branch `master` và chỉ cho maintainer push vào, trong các dự án lớn, thì chỉ set PM + CM(*) là `maintainer`, còn members bình thường thì quyền `developer` là ổn.
2. tag trong master thì phải tương ứng với `tên-của-deliverable-đã-gửi-cho-khách-hàng`  
  `tag` trong các branch thì tùy ý, ai thích đặt sao thì đặt,  
  `tag` có thể là `v1.0`, `v2.0`... hoặc `deliver-0405`, `deliver-0512`... tùy, miễn khi khách bảo "tôi phát hiện một cái lỗi trong phiên bản xx` thì các bạn biết được rằng cái lỗi đó phát sinh ở đâu mà fix, vì có những lúc, như ví dụ trong hình dưới của tôi, lúc tôi đã deliver v4.0 thì khách lại bảo "cái 4.0 tao chưa nghiệm thu đâu, nhưng lúc check source của cái 2.0 thì tao thấy 1 bug" 😑 
3. LUÔN LUÔN `merge` source từ master vào branch của mình và xử lý conflict **trước ** khi tạo **merge request**  
  **merge request** tùy tiện rồi lòi ra 1 đống conflict rồi bị CM chửi thì đừng hỏi tại sao 😅


![git-branch-guide.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1586070119101/isq7jBopX.png)

```
(*) CM: Configuration Manager: người quản lý các tài liệu, thiết bị, phiên bản trong dự án 
(*) PM: Project Manager, trong dự án nhỏ, thì PM ôm luôn việc CM. 
```