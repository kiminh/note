# module 分包介绍

1. `tf.nn`: 封装了神经网络提供各种 [激活函数](https://en.wikipedia.org/wiki/Activation_function) 和 神经网络过程中的计算。
    - 如: `tf.nn.conv1d/2d/3d/...` 各种卷积操作, `tf.nn.*pool` 各种池化操作, `tf.nn.relu/segmoid/...` 各种激活函数, `tf.nn.*_l2__*loss` 各种损失函数等。
2. `tf.estimator`: 建立模型的高级工具。 提供 `LinearRegressor`, `LinearClassifier`, `DNNRegressor`, `DNNClassifier` 等工具
3. `tf.train`: 提供训练模型。 包括 `Optimizer`, `Checkpoint`, `Feature`, `Session` 等。
    1. `xxxOptimizer extendent Optimizer`: 为各种优化器, 同时提供其 `run()` 方法执行训练。

# 使用注意事项

1. 在导入 `tensorflow` 后注意配置日志级别
2. 不论是使用高级 API 还是基础 API, 手动配置生成日志路径。该日志同事可用于 `tensorboare` 来分析 learner 的各项指标。最好是存储在统一的路径下, 以便 `tensorboard` 扫描更新。
    > 1. 使用高级 API 可以配置 `model_dir` 参数来设置日志输出路径。
    > 2. 基础 API 可以通过 `tf.summary.FileWriter` 来配置生成日志路径。 
3. `tensorboard` 有两种链接:
    1. 数据依赖: 两个 `op` 之间的 `tensor` 关系。 并且显示为实箭头。
    2. 控制依赖: 显示为虚线。
    > 图例:
    > 符号|描述
    > ---|---
    > ![namespace](imgs/namespace_node.png) | 高级节点, 双击查看高级节点结构
    > ![horizontal stack](imgs/horizontal_stack.png) | 没有连接到任何其他节点的节点编号
    > ![vertical](imgs/vertical_stack.png) | 连接到其他节点的节点编号
    > ![operation node](imgs/op_node.png) | 个性化操作节点
    > ![constant](imgs/constant.png) | 常量
    > ![summary](imgs/summary.png) | j节点总结
    > ![dataflow edge](imgs/dataflow_edge.png) | operations 之间的数据流
    > ![control edge](imgs/control_edge.png) | oerations 之间的 control dependency
    > ![reference edge](imgs/reference_edge.png) | 输出 operation 可以改变输入张量
4. 根据目前情况, 可以选用 Hadoop 作为发布平台(参考[TensorFlow on Hadoop](https://www.tensorflow.org/deploy/hadoop))。
    > 发布方式:
    > 1. Distributed TensorFlow
    > 2. TensorFlow on Hadoop
    > 3. TensorFlow on S3
    > 4. Deploy to JavaScript
5. 
