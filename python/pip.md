 1. pip install -i https://pypi.doubanio.com/simple/
 2. 在代码中使用 pip 安装相应的模块

 ```python
import pip
# 安装某个模块
pip.main(['install', '<package>'])
# 查看已经安装的模块
pip.main(['freeze'])
# 同 pip 参数
 ```

3. pip error 'ascii'

change the encode to 'utf-8' or 'gbk'

4. 国内源

1. https://pypi.tuna.tsinghua.edu.cn/simple
  - 清华大学的pip源, 是官网pypi的镜像, 每隔5分钟同步一次
2. https://pypi.douban.com/simple/
3. https://pypi.mirrors.ustc.edu.cn/simple/
4. http://pypi.mirrors.ustc.edu.cn/simple/
5. http://mirrors.aliyun.com/pypi/simple/
