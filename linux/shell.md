# shell

## 参数相关

http://blog.csdn.net/splenday/article/details/50570917

```bash
# $ 开头的变量
$# # 是传给脚本的参数个数
$0 # 是脚本本身的名字
$1 # 是传递给该shell脚本的第一个参数
$2 # 是传递给该shell脚本的第二个参数
$@ # 是传给脚本的所有参数的列表
$* # 是以一个单字符串显示所有向脚本传递的参数，与位置变量不同，参数可超过9个
$$ # 是脚本运行的当前进程ID号
$? # 是显示最后命令的退出状态，0表示没有错误，其他表示有错误
```

```bash
# test.sh
# 缺省参数设置
echo ${1:-hello}

# console:
# sh test.sh
# output: hello
# sh test.sh <parameter1>
# output <parameter1>
```

```bash
COMMAND=$1
case $COMMAND in
  # usage flags
  --help|-help|-h)
    echo "help_text"
    exit
    ;;

esac
```

## exit 方式

通过设置 exit <num> 来设置脚本执行结束的返回

```shell
...
exit 0 # 1
# 后面的指令将不被执行
...
```

## test 条件判断:

http://blog.csdn.net/cjsafty/article/details/6669755

1. test文件测试

```bash
-b file     # 若文件存在且是一个块特殊文件，则为真
-c file     # 若文件存在且是一个字符特殊文件，则为真
-d file     # 若文件存在且是一个目录，则为真
-e file     # 若文件存在，则为真
-f file     # 若文件存在且为一个规则文件，则为真
-g file     # 若文件存在且设置了SGID位的值，则为真
-h file     # 若文件存在且为一个符号链接，则为真
-k file     # 若文件存在且设置了“sticky”位的值，则为真
-p file     # 若文件存在且为一已命名管道，则为真
-s file     # 若文件存在且其大小大于零，则为真
-u file     # 若文件存在且设置了SUID位的值，则为真
-r file     # 若文件存在且可读，则为真
-w file     # 若文件存在且可写，则为真
-x file     # 若文件存在且可执行，则为真
-o file     # 若文件存在且被有效用户ID所拥有，则为真
```

2. test字符串比较

```bash
-z string         # 若string长度为0，则为真
-n string         # 若string长度不为0，则为真
string1 = string2    # 若两个字符串相等，则为真
string1 != string2   # 若两个字符串不相等，则为真
```

3. test命令的数字比较操作符

```bash
int1 -eq int2      # 若int1等于int2，则为真
int1 –ne int2      # 若int1不等于int2，则为真
int1 –lt int2      # 若int1小于int2，则为真
int1 –le int2      # 若int1小于等于int2，则为真
int1 –gt int2      # 若int1大于int2，则为真
int1 –ge int2      # 若int1大于等于int2，则为真
```

4. test复合表达式

```bash
! expr                   # 若expr为假则复合表达式为真。expr可以是任何有效的测试表达式
expr1 -a expr2           # 若expr1和expr2都为真，则为真
expr1 -o expr2           # 若expr1和expr2有一个为真，则为真
```

## 获取一条命令在 console 的输出结果

```shell
# 获取系统中关于Oracle的进程的信息
var=`ps -ef | grep oracle`
# 或者
var=$(ps -ef | grep oracle)
```
