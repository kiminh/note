# tensorflow

- [简介](#简介)
    - [基本介绍](#基本介绍)
        - [数据集导入及处理](#数据集导入及处理)
        - [机器学习基本工具](#机器学习基本工具)
        - [分布式计算平台](#分布式计算平台)
        - [分析工具 TensorBoard](#分析工具-tensorboard)
        - [Eager Execution](#eager-execution)
        - [模型的保存与恢复](#模型的保存与恢复)
- [基本使用](#基本使用)

## 简介

TensorFlow™ 是一个开放源代码软件库, 用于进行高性能数值计算。借助其灵活的架构, 用户可以轻松地将计算工作部署到多种平台 (CPU、GPU、TPU) 和设备 (桌面设备、服务器集群、移动设备、边缘设备等)。 TensorFlow™ 最初是由 Google Brain 团队 (隶属于 Google 的 AI 部门) 中的研究人员和工程师开发的, 可为机器学习和深度学习提供强力支持, 并且其灵活的数值计算核心广泛应用于许多其他科学领域。

- 相关地址:
    - [官网](https://www.tensorflow.org)
    - [接口文档](https://www.tensorflow.org/api_docs/)
    - [入门](https://www.tensorflow.org/get_started/premade_estimators)
    - [调试文档](https://www.tensorflow.org/programmers_guide/debugger)
    - [指南](https://www.tensorflow.org/programmers_guide/)
    - [机器学习速成课程](https://developers.google.com/machine-learning/crash-course/), 即 Google 机器学习公开课程 MLCC。
    - [Colaboratory](https://colab.research.google.com) 免费 `Jupyter notebook` 环境。

### 基本介绍

TensorFlow 提供了完整的包含数据集导入及处理, 机器学习基本工具, 分布式计算平台, 分析工具 `TensorBoard`, `Eager Execution`, 模型的保存与恢复 等。

除了对 `Python` 的支持, TensorFlow 还提供了 `Java`, `JavaScript`, `C++`, `Go` `Swift` 的支持, **需要注意的是: 除了 `Python` 之外的语言中的 `API` 还没有包含在 `API` 稳定承诺中。详情查看 [TensorFlow Version Compatibility](https://www.tensorflow.org/guide/version_compat)**。 同时社区中也有对 `C#`, `Haskell`, `Jilia`, `Rust`, `Scala` 等语言的支持。

*一些相关资料:*

1. TensorFlow 提供将 TensorFlow 与其他开源框架即成的最小实例 [ecosystem](https://github.com/tensorflow/ecosystem.git), 包含与以下环境与框架集成:
    - docker
    - [kubeflow](https://github.com/kubeflow/kubeflow)
    - kubernetes: 在 TensorFlow 下运行分布式 TensorFlow 的模版。
    - [marathon](https://github.com/mesosphere/marathon.git): 在 `Mesos/Marathon` 环境下使用 TensorFlow
    - hadoop
    - spark

2. TensorFlow 中实现的不同模型 [models](https://github.com/tensorflow/models.git)

3. 为机器学习提供服务的软件库 [serving](https://github.com/tensorflow/serving.git)

4. oracle 开源的用于简化机器学习模型部署, 并将其与特定框架模型实现分离开的 [GraphPipe](https://github.com/oracle/graphpipe.git), 提供对 `TensorFlow`, `PyTorch` 等的支持, 统一向外部应用提供服务。 该项目目前并不完善。

#### 数据集导入及处理

`tf.data` 主要用于从简单的, 可崇勇的部分构建复杂的输入管道。 如: 图像模型的管道可能从文件中聚合数据, 对每个图像应用随机扰动, 并将随机选择的数据图像合并到一个批处理中进行训练; 文本模型管道可能包括从原始文本数据中提取符号, 将其转换成嵌入标志符的查找表, 并将不同长度的序列组合在一起。

#### 机器学习基本工具

1. 提供了 `tf.estimator`, 高度简化机器学习编程的高级 API, 封装了 `training`, `eva;iatopm`, `prediction`, `export for serving` 等接口用于简化机器学习编程。 其中包括 `LinearRegressior`, `LinearClassifier`, `DNNRegressior`, `DNNClassifier` 等工具。

2. 提供了一系列的基础 API(TensorFlow Core), 其中包括 `tf.Graph`, `tf.Session`, `tf.placeholder`, `tf.data` 提供的相关 API, `tf.layers` 提供向 `tf.Graph` 中添加可训练参数的首选方法, `tf.feature_column` 用于特征处理。 官方建议尽可能的使用高级 API 来构建模型, 但是了解 TensorFlow 的核心有如下好处:
    1. 使用基础 API 操作时, 试验和调试都更直接。
    2. 提供了一个在使用高级 API 时, 内部工作方式的理想模型。

#### 分布式计算平台

这里主要包含两块内容: `tf.Server` 和 `tf.train.ClusterSpec`。 可以通过这两部分来构建基础的分布式 TensorFlow 平台。

- 本地 TensorFlow Server 简单构建:

```python
#!/usr/bin/env python

from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import tensorflow as tf

server = tf.train.Server.create_local_server()
```

- 集群简单构建:

```python
#!/usr/bin/env python

from __future__ import absolute_import, division, print_function

import tensorflow as tf

cluster = tf.train.ClusterSpec({
    "worker": [
        "worker0.example.com:2222",
        "worker1.example.com:2222",
        "worker2.example.com:2222"
    ],
    "ps": [
        "ps0.example.com:2222",
        "ps1.example.com:2222"
    ]})
# 启动 worker 分组中的第一个 Server worker0.example.com:2222
server = tf.Server(cluster, job_name="worker", task_index=0)
```

- TensorFlow 集群构建还可以通过 `tensorflow.core.protobuf` 和 `tensorflow.python.platform` 来构建, 参考 [grpc_tensorflow_server.py](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/dist_test/server/grpc_tensorflow_server.py)

#### 分析工具 TensorBoard

TensorBoard 可以直观的展现 TensorFlow 图, 绘制图像生成的定量指标图以及显示附加数据。 无需单独安装。 关于这一部分可以参考 [TensorBoard：可视化学习](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard)。

#### Eager Execution

TensorFlow 的 Eager Execution 是一种命令式编程环境, 可立即评估操作, 无需构建图: 操作会返回具体的值, 而不是构建以后再运行的计算图。 Eager Execution主要用于研究与实验, 有以下优点:

- 自然的组织代码结构, 并使用 Python 数据结构, 能够快速迭代小模型和小型数据集。
- 直接调用操作一检查正在运行的模型, 并测试更改, 使用标准的 Python 调试工具进行即时错误报告。
- 使用 Python 控制流程而不是 `tf.Graph` 控制流程。

启用方式:

```python
from __future__ import abslute_import, division, print_function
import tensorflow as tf

tf.enable_eager_execution()
```

更详细的解释参考[Eager Execution](https://www.tensorflow.org/programmers_guide/eager)。

#### 模型的保存与恢复

TensorFlow 中, 变量是表示程序操作的共享持久状态的最佳方法。 `tf.train.Saver` 针对 Graph 中所有变量或指定列表的变量将 `save` 和 `restore` 操作添加到 Graph 中, Server 提供运行这些操作的方法, 并指定写入或读取 Checkpoint 文件路径。

`tf.train.Saver` 提供保存模型和恢复模型的方法, 通过 `tf.saved_model.simple_save` 函数保存适合投入使用的模型。 Estimator 会自动保存和恢复 `model_dir` 中的变量。

#### s
