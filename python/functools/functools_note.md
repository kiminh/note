# functools

## function.partial

- 作用:

通过包装的手法, 允许 "重新定义" 函数签名。  
用一些默认参数包装一个可调用对象, 返回结果可以是可调用对象, 并像原始对象一样对待。  

- 用法: 

```python
def partial(func, *args, **keywords):
    def newfunc(*fargs, **fkeywords):
        newkeywords = keywords.copy()
        newkeywords.update(fkeywords)
        # 合并, 调用原始函数, 使用 partial 参数
        return func(*(args + fargs), **newkeywords)

    newfunc.func = func
    newfunc.args = args
    newfunc.keywords = keywords
    return newfunc
```

声明:

```
urlunquote = functools.partial(urlunquote, encoding='latin1')
```
