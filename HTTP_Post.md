# HTTP Post

http协议中post请求提交数据大概有6中形式。通过在POST请求头部设置Content-Type:来决定采用那种格式的数据编码方式。

常用的数据提交方式有。

1. **application/x-www-form-urlencoded**
2. **multipart/form-data**
3. **application/json**
4. **text/plain**
5. **application/xml**
6. **application/octet-stream**



## application/x-www-form-urlencoded

x-www-form-urlencoded其实是一种编码类型。浏览器用x-www-form-urlencoded的编码类型把form数据转换成一个字串（name1=value1&name2=value2…），然后把这个字串append到url后面，用?分割，加载这个新的url。POST:  浏览器把form数据封装到http body中，然后发送到server。

```
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 39

username=john_doe&password=secret123&age=30
```

## multipart/form-data

该编码类型，也被成为Media类型方式多用来上传文件。



