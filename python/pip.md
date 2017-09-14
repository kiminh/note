 1. .\pip.exe install -i https://pypi.doubanio.com/simple/
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
