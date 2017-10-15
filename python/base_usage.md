# python base usage for more effecting

1. yield

yield 作用主要是执行异步流程, 生成大 List, 遍历大 List, 文件时可以提升执行效率。

```python
def analize(list):
  for element in list:
    yield element

def execute():
  for input in analize:
    # executions
    pass

if __name__ == '__main__':
  execute()
```
