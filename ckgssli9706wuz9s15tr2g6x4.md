## Trong winform, cái object sender , EventArgs e nghĩa là gì

Sáng nay gặp 1 bài hay trên 1 Group Lập trình trên FB, hỏi một thứ mà một LTV thực sự nên hỏi: 

> hai đối số này có ý nghĩa gì vậy mọi người? 

> ![winform-sender-eventargs.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1603852589878/ORZSVwMMW.jpeg)

trả lời :

chỗ này để hiểu rõ hơn bạn cần xem file xx.Designer.cs của cái form nữa.

khi bạn "kéo thả" 1 control (button/checkbox ..) vào form, bản chất là Visual Studio sẽ giúp bạn viết mấy dòng code này,

```cs
this.button1 = new System.Windows.Forms.Button();
this.SuspendLayout();
// 
// button1
// 
this.button1.Location = new System.Drawing.Point(176, 61);
this.button1.Name = "button1";
this.button1.Size = new System.Drawing.Size(75, 23);
this.button1.TabIndex = 0;
this.button1.Text = "button1";
this.button1.UseVisualStyleBackColor = true;
this.button1.Click += new System.EventHandler(this.button1_Click);
```
trong đó chú ý dòng:

```cs
this.button1.Click += new System.EventHandler(this.button1_Click);
```

`this.button1_Click` chính là cái hàm mà bạn sẽ viết thêm ruột để xử lý bài toán của mình khi button 1 được clicked.

như tên, `EventHandler` là cái mà sẽ xử lý (handle) các sự kiện (event) diễn ra trong form

khi `button1` được `click`, `EventHandler` tương ứng với nó sẽ được kích hoạt, trong đó, cái `object sender`, nó chính là cái object "trỏ vào" button này. 

EventArgs thì đại diện cho cái data liên quan tới event đó, và có thể được sử dụng để truyền tham số (parameters), thông tin trạng thái (state information) hoặc data liên quan, ví dụ như khi 1 button được click, bạn có thể tìm được tọa độ của button đó bằng cách "ép kiểu" (cast) cái EventArgs thành MouseEventArgs

```cs
private void button1_Click(object sender, EventArgs e)
{
    MouseEventArgs e2 = (MouseEventArgs)e;
    MessageBox.Show(string.Format("X: {0} Y: {1}", e2.X, e2.Y));
}
```

hoặc, trong một số tình huống, bạn muốn cancel sự kiện đi, vậy ép nó thành CancelEventArgs

```cs
private void HandleCancellableEvent(object sender, EventArgs e)
{
    if(/* điều kiện*/)
    {
        CancelEventAgrs e2 = (CancelEventAgrs )e;
        // Cancel this event
        e2.Cancel = true;
    }
}
```
