# 字符流
char=16bit

Reader、Writer ，使用缓冲区，不关闭流不输出任何内容
# 字节流
byte=8bit

OutputStream、InputStream，不用缓冲区


OutputStreamWriter：是Writer的子类，将输出的字符流变为字节流
InputStreamReader：是Reader的子类，将输入的字节流变为字符流


# IO模型
## 阻塞式IO
BIO。
data = socket.read();
阻塞时，会交出CPU。
适合连接数目比较小且固定
## 非阻塞式IO
![](../pic/Image2.png)
不会交出CPU,反而会一直占用CPU
## IO复用模型
NIO就是多路复用IO。
IO处理流，NIO处理块。
适合连接数目多且链接短。
## 信号驱动IO模型
## 异步IO模型
AIO
适合连接数目多且连接长