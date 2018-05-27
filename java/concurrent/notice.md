# java 线程使用注意

- 来自 [15个高级Java多线程面试题及回答](https://mp.weixin.qq.com/s?__biz=MzA4NDc2MDQ1Nw==&mid=2650238379&idx=1&sn=4ea339e4b2179628fee851d50696a269&chksm=87e18f4db096065b0bec23b7363dea0b2f50fbd39a480a2e1d2a4d058bc1dc5dd807eaa2a0f7&scene=21#wechat_redirect)

1. wait() vs sleep()
  - wait() 在等待时会释放锁, 通常用于线程交互。
  - sleep() 在等待时一直有锁, 通常用于暂停执行。
