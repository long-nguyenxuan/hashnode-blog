## Hướng dẫn áp dụng Git flow trong dự án

## Vấn đề 

Khi làm dự án phần mềm, việc nhiều người cùng edit chung 1 file và sử dụng các tool version control để quản lý các phiên bản gần như là không thể tránh khỏi, `git` và các ứng dụng mở rộng của nó (github/gitlab/bitbucket) đang hầu như thống trị tuyệt đối mảng này.
Mặc dù là 1 công cụ rất mạnh được sinh ra để giải quyết vấn đề, nhưng nếu cách sử dụng không đúng, thì việc sử dụng git lại có thể gây ra rất nhiều vấn đề 😄 có nhiều best practice có thể áp dụng để tránh giẫm chân nhau, nổi tiếng nhất có thể kể đến `git-flow` của bác [Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/), mà sau đó Github và Gitlab chế lại của bác ấy thành [Github Flow](https://guides.github.com/introduction/flow/) và [GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html)  

Bên dưới là 1 trong những cách như vậy, cái này lấy ý tưởng từ git-flow, và điều chỉnh một chút cho phù hợp với quy trình ở công ty mình đang làm việc, hy vọng giúp ích cho các bạn 

## Pre-condition

Đã có khái niệm cơ bản về version control và git 

## Target audience 

All member trong dự án dự định sử dụng Git, hoặc đang áp dụng Git và thấy nó "chẳng khác gì xx" 

## Giả định 

Dự án có áp dụng các môi trường theo mô hình tương tự như thế này: 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604975343007/3oTRGyD_l.png)

Trong đó 

- Test environment 
  là môi trường của team tester nội bộ 
- Staging environment 
  là môi trường cho khách hàng nghiệm thu 
- Production environment 
  là môi trường vận hành 

## Core Idea

1. nhánh `master` CHỈ chứa source production, 
  nói cách khác, tất cả các phiên bản trong nhánh `master` phải là _deployable_
  nghĩa là khi có chuyện xảy ra với phiên-bản-vừa-được-deploy, bạn chỉ đơn giản là nhảy-lui 1 version và re-deploy 
2. nhánh `staging` chứa source **chờ nghiệm thu** (*) 
3. nhánh `testing` chứa source **đang được tester chạy test** 
4. nhánh `develop` chứa **source ngon** đang chờ dồn 1 cục rồi đưa lên `testing` 
3. mỗi members push code hàng ngày vào `feature/{username}/{id}-feature-name` hoặc `fix-bug/{username}/{id}-bug-subject` 
4. nhánh `hotfix/{id}-bug-subject` chỉ để xử lý bug production "thực-sự-hot" và nên do người chuyên trách hoặc người có trách nhiệm cao nhất về code trong dự án xử lý, tránh các lỗi bất cẩn không đáng có. 

## Nguyên tắc 

1. **KHÔNG BAO GIỜ** `push` vào `master`  
  trong master chỉ chứa source được merge.  
  note: để phòng tránh việc này, gitlab nó default protect luôn cái branch `master` và chỉ cho maintainer push vào, trong các dự án lớn, thì chỉ set PM + CM(*) là `maintainer`, còn members bình thường thì quyền `developer` là ổn.
2. tag trong master thì phải tương ứng với `tên-của-deliverable-đã-gửi-cho-khách-hàng`  
  - `tag` trong các branch thì tùy ý, thích đặt sao thì đặt, nhưng nên có convention thống nhất trong dự án 
  - `tag` có thể là `v1.0`, `v2.0`... hoặc `deliver-0405`, `deliver-0512`... tùy 
  miễn khi khách bảo "tôi phát hiện một cái lỗi trong phiên bản xx` thì các bạn biết được rằng cái lỗi đó phát sinh ở đâu mà fix, vì có những lúc, như ví dụ trong hình dưới của tôi, lúc tôi đã deliver v4.0 thì khách lại bảo "cái 4.0 tao chưa nghiệm thu đâu, nhưng lúc check source của cái 2.0 thì tao thấy 1 bug" 😑 
3. LUÔN LUÔN `merge` source từ `develop` vào branch của mình và xử lý conflict **trước ** khi tạo **merge request**  
  **merge request** tùy tiện rồi lòi ra 1 đống conflict rồi bị CM chửi thì đừng hỏi tại sao 😅 

## Tag convention 

- phiên bản để gửi cho tester test, thì nằm trong nhánh `testing` và có tag name là `test-v{version number}`, ví dụ `test-v3.0.1` hoặc `test-20201110`, thường thì đánh version kiểu `x.y.z` nhìn nó "chuyên nghiệp" hơn, đồng thời [có ý nghĩa hơn](https://semver.org/) 
- phiên bản để gửi cho khách hàng nghiệm thu (trong trường hợp dự án có môi trường `staging`) nên có tag name là `uat-v{version number}`, ví dụ: `UAT-v3.0.1` 
- phiên bản để gửi lên môi trường production, nằm trong nhánh `master` và có tag name là `release-v{version number}`, ví dụ: `release-v3.0.1` 

mấy cái convention này nếu như dự án nào áp dụng CI/CD thì có thể dùng để trigger build node / intergration node chạy luôn, tuy nhiên đấy là câu chuyện khác không nằm trong scope của post này

## Customize 

### Dự án 1 người 

Trong một số dự án siêu nhỏ & đơn giản, ví dụ như "dự án 1 người" thì bạn cũng có thể trực tiếp push vào master cho đỡ việc, tuy nhiên việc gắn tag name thì nên tuân thủ convention trên, ít nhất là có 2 tag: `UAT-v3.0.1` và `release-v3.0.1` được gắn cùng vào 1 `commit hash` trên `master` nếu như khi gửi UAT và release và khách hàng confirm "không có lỗi gì" 

### không có tester 

Đôi khi, trong một số công ty nhỏ, làm app nội bộ, "khách hàng" lúc này đồng thời là tester, vậy thì có khi cũng không cần phải tạo thêm nhánh `develop` & `testing` gì cho rách việc, chỉ cần `feature/*`, `staging`, `master` là OK 

đương nhiên, nếu làm chuẩn và sử dụng hotkey/script tinh tế, thì cho dù có đủ 4 branch, bạn vẫn không tốn quá nhiều công sức cho các thao tác này, mà việc control 1 bộ source ngăn nắp sẽ giúp bạn tiết kiệm effort hơn nhiều 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1605027984317/r2F1-4fNG.png)


```
(*) staging: môi trường hoàn toàn giống production, thường dùng trong các dự án 
(*) CM: Configuration Manager: người quản lý các tài liệu, thiết bị, phiên bản trong dự án 
(*) PM: Project Manager, trong dự án nhỏ, thì PM ôm luôn việc CM. 

```

