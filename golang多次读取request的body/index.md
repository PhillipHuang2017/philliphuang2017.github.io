# Golang多次读取Request的Body


# Golang中间件多次读取Request的Body

## 问题描述

在用echo写中间件的时候需要提取Body的内容进行验证，但是Body是一个`io.ReadCloser`，即要求实现`Read()`和`Close()`方法的`interface`，也就是说也是个`Reader`接口，而`Reader`接口读完就没了，类似于从TCP流中读数据那样。如果中间件把Body读完了的话，后面的handler就没法读了。

看`Request`结构体的源码看到里面有个`GetBody`成员，是个函数，该函数返回Body的副本，但是试了一下发现会报错，非法的内存什么的，网上有人说这个是用在客户端的，不是用在服务端的，没在客户端试过。

然后源码里面`NewRequestWithContext`函数中给`GetBody`赋值时`GetBody`函数中是通过`ioutil.NopCloser`来创建`Request`的，而`ioutil.NopCloser`结构体中实现的`Close()`函数实际上啥也不做，直接返回`nil`，所以关闭Body到底有啥用？

stackoverflow也有人提这个问题。

## 解决方案

后来在stackoverflow找到了[解决方案](https://stackoverflow.com/questions/43021058/golang-read-request-body)，先把body读出来，再重新给Request写入一个body。简单的代码如下：

```go
bodyBytes, err := ioutil.ReadAll(req.Body)
if err!=nil {
	return err
}
req.Body.Close()  //  must close
req.Body = ioutil.NopCloser(bytes.NewBuffer(bodyBytes))
```

至于用不用关闭Body以及关闭了有什么效果还需要后面再了解（按道理来讲应该是需要关闭的，因为实现了`Closer`接口，但Body又是个`ioutil.NopCloser`，其`Close()`函数不做任何事情）。

## 参考

1.[Golang read request body](https://stackoverflow.com/questions/43021058/golang-read-request-body)

2.[How to read request body twice in Golang middleware?](https://stackoverflow.com/questions/46948050/how-to-read-request-body-twice-in-golang-middleware)
