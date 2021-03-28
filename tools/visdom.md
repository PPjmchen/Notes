## 端口占用问题解决

**Visdom 解决OSError: [Errno 98] Address already in use)**

lsof -i tcp:8097 查看占用该端口的PID
kill -9 22215 杀死该PID
lsof -i tcp:8097 检查一下是否仍被占用

