# RSA 加密算法

RSA 是一种非对称加密算法。

在公开密钥密码体制中, 加密密钥(Public Key)是公开信息, 解密密钥(`secret key encryptuib`) 是需要保密的, 也就是经常看见的 `Private Key`。 加密算法 E 和解密算法 D 同样也是公开的。 解密密钥 SK 是由公钥 PK 决定, 由于无法计算出 大数 n 的欧拉函数($\varphi(n)$), 因此不能根据 PK 计算出 SK。

- RSA  算法:

通常先生成一堆 RSA 密钥, 密钥由用户保存, 公钥可对外公开。 为提高保密强度, 密钥长度至少 500 位, 一般推荐使用 1024 位。常采用传统加密方法和公开加密方法结合的方式, 即信息采集采用改进的 DES 或 IDEA 密钥加密, 然后使用 RSA 密钥加密对话密钥和信息摘要, 收到信息后通不同的密钥解密并可核对信息摘要。

## 实现过程

- [参考](http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html)

1. 随机选择两个不想等的质数 `p`, `q`。
2. 将两数相乘得到 `n`。
3. 计算 `n` 的欧拉函数 $\varphi(n)$:
$$\varphi(n)=(p-1)(q-1)$$
4. 随机选择一个整数 $e$ 满足 $1<e<\varphi(n)$, 且 $e$ 与 $\varphi(n)$ 互质。
5. 计算 $e$ 对于 $\varphi(n)$ 的模反元素 $d$:
$$ed\equiv1(\mod\varphi(n))$$
等价于:
$$ed-1=k\varphi(n)$$
6. 将 $n$ 和 $e$ 封装成公钥, $n$ 和 $d$ 封装成私钥。

## 填充模式

RSA 在一个固定长度的块上进行操作, block length 和 key length 使用的填充模式有关。

- `RSA_PKCS1_PADDING` 填充模式

    - 要求
    > 输入必须比 RSA 钥模(modulus) 短至少 11 个字节, 也就是 `RSA_size(rsa) - 11`。 如果输入的明文过长, 必须切割, 然后填充。
    - 输出
    > 和 `modulus` 一样长

- RSA_PKCS1_OAEP_PADDING

`RSA_size(rsa) - 41`

- for RSA_NO_PADDING 不填充。
