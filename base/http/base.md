# HTTP Base Learning

##

## GET vs POST

1. GET

2. POST

- 比较

比较项 | GET | POST
---|---|---
刷新 | 无害 | 数据被重新提交
缓存 | 能被缓存 | 不能缓存
书签 | 可以收藏为书签 | 不能收藏为书签
编码类型 | application/x-www-form-urlencoded | application/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码
历史 | 参数保留在浏览器历史中 | 参数不会保留在浏览器历史中
对数据长度的限制 | URL 的长度是受限制的, URL 的最大长度是 2048 个字符 | 无限制
数据类型的限制 | 只允许 ASCII 字符 | 没有限制, 允许二进制数据
安全性 | 与 POST 相比，GET 的安全性较差, 因为所发送的数据是 URL 的一部分。**在发送密码或其他敏感信息时绝不要使用 GET** | POST 比 GET 更安全, 因为参数不会被保存在浏览器历史或 web 服务器日志中
可见性 | 数据在 URL 中对所有人都是可见的 | 数据不会显示在 URL 中
