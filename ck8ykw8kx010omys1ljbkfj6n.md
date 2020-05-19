## "lạm dụng design pattern"

// source: https://qr.ae/pNrvfZ 

## Hỏi: 

Vì sao mấy anh senior bảo tôi rằng "design pattern phổ biến qua nên giờ tụi dev non lạm dụng design pattern dữ quá" ? 

## Trả lời: 

Design pattern là 1 cái "tục ngữ" mà bọn kiến trúc sư phần mềm (software architect) sử dụng để mô tả thiết kế với nhau. 
Design pattern không phải là 1 bộ quy tắc để quyết định bạn sẽ code cái gì. 

Thử so sánh 2 đoạn mô tả sau: 

> Tôi sẽ giấu `constructor` của cái class này đi, và thay vì đó viết 1 cái `helper class` mà tập hợp các tham số hợp lệ cho class này, và khi toàn bộ các `field` cần thiết được `set`, cái helper class này sẽ cho phép bạn khởi tạo `class này`. 

với 

> Tui sẽ làm 1 cái  [builder ](https://www.geeksforgeeks.org/builder-design-pattern/) cho class này 

Nếu bạn quen thuộc với `Builder pattern`, câu thứ hai là rõ ràng là súc tích hơn rất nhiều, nếu bạn không quen thuộc, cũng không sao cả, mấy anh pro đấy sẽ giải thích cho bạn. Điều đó giúp kéo mọi người lên cùng nhau đứng trên vai người khổng lồ thay vì lẩn quẩn với những khái niệm căn bản. 

Tuy nhiên, thông thường bọn **dev non** sau khi được học về builder pattern sẽ nói với bạn như này: 

> oh, **trên mạng** có 1 thứ gọi là `builder pattern` để build class, nên nếu chú mày trưng (expose) cái `constructor` trực tiếp ra như vậy là **sai**. 

éo phải! Một dev già sẽ bảo bạn tạo `builder` khi bạn **cần phải** dùng đến `builder`. Cái constructor-có-mỗi-2-tham-số của bạn thì không cần `builder` làm gì cả. Còn cái constructor-có-47-tham-số dính tới một tấn data và workflow phức tạp thì cần 1 cái `builder` (hoặc cần xem lại thiết kế của bản thân cái class - nhưng đấy là 1 câu chuyện khác). 

Coi design pattern là "kinh thánh" thay vì một đống "tục ngữ" chính là sự lạm dụng mà anh dev già đó nói. 
