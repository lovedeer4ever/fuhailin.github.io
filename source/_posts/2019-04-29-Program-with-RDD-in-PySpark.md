---
title: Spark入门笔记—编程操作对象RDD与DataFrame(PySpark版)
date: 2019-04-29 14:56:00
tags: [Spark,Python]
categories: 大数据
top:
---
# Introduction
RDD（Resilient Distributed Dataset）叫做**弹性分布式数据集**，
在之前的[Spark基本概念](https://fuhailin.github.io/Spark-Tutorial/#RDD-Resilient-Distributed-Dataset)当中我已经介绍过RDD是Spark中最基本的数据结构，是一个不可变的分布式对象集合。Spark的核心是建立在统一的抽象RDD之上，使得Spark的各个组件可以无缝进行集成，在同一个应用程序中完成大数据计算任务。RDD的设计理念源自AMP实验室发表的论文《Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing》。
<!-- more -->

# Background
在实际应用中，存在许多迭代式算法（比如机器学习、图算法等）和交互式数据挖掘工具，这些应用场景的共同之处是，不同计算阶段之间会重用中间结果，即一个阶段的输出结果会作为下一个阶段的输入。但是，目前的MapReduce框架都是把中间结果写入到HDFS中，带来了大量的数据复制、磁盘IO和序列化开销。虽然，类似Pregel等图计算框架也是将结果保存在内存当中，但是，这些框架只能支持一些特定的计算模式，并没有提供一种通用的数据抽象。RDD就是为了满足这种需求而出现的，它提供了一个抽象的数据架构，我们不必担心底层数据的分布式特性，只需将具体的应用逻辑表达为一系列转换处理，不同RDD之间的转换操作形成依赖关系，可以实现管道化，从而避免了中间结果的存储，大大降低了数据复制、磁盘IO和序列化开销。

## RDD概念
一个RDD就是一个分布式对象集合，本质上是一个只读的分区记录集合，每个RDD可以分成多个分区，每个分区就是一个数据集片段，并且一个RDD的不同分区可以被保存到集群中不同的节点上，从而可以在集群中的不同节点上进行并行计算。RDD提供了一种高度受限的共享内存模型，即RDD是只读的记录分区的集合，不能直接修改，只能基于稳定的物理存储中的数据集来创建RDD，或者通过在其他RDD上执行确定的转换操作（如map、join和groupBy）而创建得到新的RDD。RDD提供了一组丰富的操作以支持常见的数据运算，分为“行动”（Action）和“转换”（Transformation）两种类型，前者用于执行计算并指定输出的形式，后者指定RDD之间的相互依赖关系。两类操作的主要区别是，转换操作（比如map、filter、groupBy、join等）接受RDD并返回RDD，而行动操作（比如count、collect等）接受RDD但是返回非RDD（即输出一个值或结果）。RDD提供的转换接口都非常简单，都是类似map、filter、groupBy、join等粗粒度的数据转换操作，而不是针对某个数据项的细粒度修改。因此，RDD比较适合对于数据集中元素执行相同操作的批处理式应用，而不适合用于需要异步、细粒度状态的应用，比如Web应用系统、增量式的网页爬虫等。正因为这样，这种粗粒度转换接口设计，会使人直觉上认为RDD的功能很受限、不够强大。但是，实际上RDD已经被实践证明可以很好地应用于许多并行计算应用中，可以具备很多现有计算框架（比如MapReduce、SQL、Pregel等）的表达能力，并且可以应用于这些框架处理不了的交互式数据挖掘应用。
Spark用Scala语言实现了RDD的API，程序员可以通过调用API实现对RDD的各种操作。RDD典型的执行过程如下：
1. RDD读入外部数据源（或者内存中的集合）进行创建；
2. RDD经过一系列的“转换”操作，每一次都会产生不同的RDD，供给下一个“转换”使用；
3. 最后一个RDD经“行动”操作进行处理，并输出到外部数据源（或者变成Scala集合或标量）。

需要说明的是，RDD采用了惰性调用，即在RDD的执行过程中（如下图所示），真正的计算发生在RDD的“行动”操作，对于“行动”之前的所有“转换”操作，Spark只是记录下“转换”操作应用的一些基础数据集以及RDD生成的轨迹，即相互之间的依赖关系，而不会触发真正的计算。
![Spark的转换和行动操作](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/Spark-Action-Transformation.jpg)
例如，在下图中，从输入中逻辑上生成A和C两个RDD，经过一系列“转换”操作，逻辑上生成了F（也是一个RDD），之所以说是逻辑上，是因为这时候计算并没有发生，Spark只是记录了RDD之间的生成和依赖关系。当F要进行输出时，也就是当F进行“行动”操作的时候，Spark才会根据RDD的依赖关系生成DAG，并从起点开始真正的计算。
![RDD执行过程的一个实例](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/RDD-execute-intence.jpg)
上述这一系列处理称为一个“血缘关系（Lineage）”，即DAG拓扑排序的结果。采用惰性调用，通过血缘关系连接起来的一系列RDD操作就可以实现管道化（pipeline），避免了多次转换操作之间数据同步的等待，而且不用担心有过多的中间数据，因为这些具有血缘关系的操作都管道化了，一个操作得到的结果不需要保存为中间数据，而是直接管道式地流入到下一个操作进行处理。同时，这种通过血缘关系把一系列操作进行管道化连接的设计方式，也使得管道中每次操作的计算变得相对简单，保证了每个操作在处理逻辑上的单一性；相反，在MapReduce的设计中，为了尽可能地减少MapReduce过程，在单个MapReduce中会写入过多复杂的逻辑。

## RDD特性
总体而言，Spark采用RDD以后能够实现高效计算的主要原因如下：
1. 高效的容错性。现有的分布式共享内存、键值存储、内存数据库等，为了实现容错，必须在集群节点之间进行数据复制或者记录日志，也就是在节点之间会发生大量的数据传输，这对于数据密集型应用而言会带来很大的开销。在RDD的设计中，数据只读，不可修改，如果需要修改数据，必须从父RDD转换到子RDD，由此在不同RDD之间建立了血缘关系。所以，RDD是一种天生具有容错机制的特殊集合，不需要通过数据冗余的方式（比如检查点）实现容错，而只需通过RDD父子依赖（血缘）关系重新计算得到丢失的分区来实现容错，无需回滚整个系统，这样就避免了数据复制的高开销，而且重算过程可以在不同节点之间并行进行，实现了高效的容错。此外，RDD提供的转换操作都是一些粗粒度的操作（比如map、filter和join），RDD依赖关系只需要记录这种粗粒度的转换操作，而不需要记录具体的数据和各种细粒度操作的日志（比如对哪个数据项进行了修改），这就大大降低了数据密集型应用中的容错开销；
2. 中间结果持久化到内存。数据在内存中的多个RDD操作之间进行传递，不需要“落地”到磁盘上，避免了不必要的读写磁盘开销；
3. 存放的数据可以是Java对象，避免了不必要的对象序列化和反序列化开销。

## RDD之间的依赖关系
RDD中不同的操作会使得不同RDD中的分区会产生不同的依赖。RDD中的依赖关系分为窄依赖（Narrow Dependency）与宽依赖（Wide Dependency），图9-10展示了两种依赖之间的区别。
窄依赖表现为一个父RDD的分区对应于一个子RDD的分区，或多个父RDD的分区对应于一个子RDD的分区；比如图9-10(a)中，RDD1是RDD2的父RDD，RDD2是子RDD，RDD1的分区1，对应于RDD2的一个分区（即分区4）；再比如，RDD6和RDD7都是RDD8的父RDD，RDD6中的分区（分区15）和RDD7中的分区（分区18），两者都对应于RDD8中的一个分区（分区21）。
宽依赖则表现为存在一个父RDD的一个分区对应一个子RDD的多个分区。比如图9-10(b)中，RDD9是RDD12的父RDD，RDD9中的分区24对应了RDD12中的两个分区（即分区27和分区28）。
总体而言，如果父RDD的一个分区只被一个子RDD的一个分区所使用就是窄依赖，否则就是宽依赖。窄依赖典型的操作包括map、filter、union等，宽依赖典型的操作包括groupByKey、sortByKey等。对于连接（join）操作，可以分为两种情况。
1. 对输入进行协同划分，属于窄依赖（如下图(a)所示）。所谓协同划分（co-partitioned）是指多个父RDD的某一分区的所有“键（key）”，落在子RDD的同一个分区内，不会产生同一个父RDD的某一分区，落在子RDD的两个分区的情况。
2. 对输入做非协同划分，属于宽依赖，如下图(b)所示。

对于窄依赖的RDD，可以以流水线的方式计算所有父分区，不会造成网络之间的数据混合。对于宽依赖的RDD，则通常伴随着Shuffle操作，即首先需要计算好所有父分区数据，然后在节点之间进行Shuffle。
![窄依赖与宽依赖的区别](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/9-10-.jpg)
Spark的这种依赖关系设计，使其具有了天生的容错性，大大加快了Spark的执行速度。因为，RDD数据集通过“血缘关系”记住了它是如何从其它RDD中演变过来的，血缘关系记录的是粗颗粒度的转换操作行为，当这个RDD的部分分区数据丢失时，它可以通过血缘关系获取足够的信息来重新运算和恢复丢失的数据分区，由此带来了性能的提升。相对而言，在两种依赖关系中，窄依赖的失败恢复更为高效，它只需要根据父RDD分区重新计算丢失的分区即可（不需要重新计算所有分区），而且可以并行地在不同节点进行重新计算。而对于宽依赖而言，单个节点失效通常意味着重新计算过程会涉及多个父RDD分区，开销较大。此外，Spark还提供了数据检查点和记录日志，用于持久化中间RDD，从而使得在进行失败恢复时不需要追溯到最开始的阶段。在进行故障恢复时，Spark会对数据检查点开销和重新计算RDD分区的开销进行比较，从而自动选择最优的恢复策略。

## 阶段的划分
Spark通过分析各个RDD的依赖关系生成了DAG，再通过分析各个RDD中的分区之间的依赖关系来决定如何划分阶段，具体划分方法是：在DAG中进行反向解析，遇到宽依赖就断开，遇到窄依赖就把当前的RDD加入到当前的阶段中；将窄依赖尽量划分在同一个阶段中，可以实现流水线计算（具体的阶段划分算法请参见AMP实验室发表的论文《Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing》）。例如，如图9-11所示，假设从HDFS中读入数据生成3个不同的RDD（即A、C和E），通过一系列转换操作后再将计算结果保存回HDFS。对DAG进行解析时，在依赖图中进行反向解析，由于从RDD A到RDD B的转换以及从RDD B和F到RDD G的转换，都属于宽依赖，因此，在宽依赖处断开后可以得到三个阶段，即阶段1、阶段2和阶段3。可以看出，在阶段2中，从map到union都是窄依赖，这两步操作可以形成一个流水线操作，比如，分区7通过map操作生成的分区9，可以不用等待分区8到分区9这个转换操作的计算结束，而是继续进行union操作，转换得到分区13，这样流水线执行大大提高了计算的效率。
![根据RDD分区的依赖关系划分阶段](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/9-11-RDD.jpg)
由上述论述可知，把一个DAG图划分成多个“阶段”以后，每个阶段都代表了一组关联的、相互之间没有Shuffle依赖关系的任务组成的任务集合。每个任务集合会被提交给任务调度器（TaskScheduler）进行处理，由任务调度器将任务分发给Executor运行。

## RDD运行过程
通过上述对RDD概念、依赖关系和阶段划分的介绍，结合之前介绍的Spark运行基本流程，这里再总结一下RDD在Spark架构中的运行过程（如图9-12所示）：
1. 创建RDD对象；
2. SparkContext负责计算RDD之间的依赖关系，构建DAG；
3. DAGScheduler负责把DAG图分解成多个阶段，每个阶段中包含了多个任务，每个任务会被任务调度器分发给各个工作节点（Worker Node）上的Executor去执行。

![RDD在Spark中的运行过程](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/9-12-RDD-Spark.jpg)

### DataFrame与RDD的区别
![DataFrame与RDD的区别](https://gitee.com/fuhailin/Object-Storage-Service/raw/master/DataFrame-RDD.jpg)
 - RDD是分布式的 Java对象的集合。比如，RDD[Person]是以Person为类型参数，但是，Person类的内部结构对于RDD而言却是不可知的。
 - DataFrame是一种以RDD为基础的分布式数据集，也就是分布式的Row对象的集合（每个Row对象代表一行记录），提供了详细的结构信息，也就是常说的模式（schema），Spark SQL可以清楚地知道该数据集中包含哪些列、每列的名称和类型。DataFrame的推出，让Spark具备了处理大规模结构化数据的能力，不仅比原有的RDD转化方式更加简单易用，而且获得了更高的计算性能。和RDD一样，DataFrame的各种变换操作也采用惰性机制，只是记录了各种转换的逻辑转换路线图（是一个DAG图），不会发生真正的计算。DAG图相当于一个逻辑查询计划，最终，会被翻译成物理查询计划，生成RDD DAG，按照RDD DAG的执行方式去完成最终的计算得到结果。

### RDD和DataFrame不同版本API执行效率比较
目前spark的编程接口中，执行效率是Scala DataFrame api > PySpark DataFrame api > Scala RDD api > PySpark RDD api
总的来说，DataFrame执行效率高于RDD，而Scala执行效率又高于Python

Dataset 上可用的操作可分为 Transformation 和 Action 两大类操作。Transformation操作会产生新的Dataset， 而Action操作将会触发计算并返回结果。Transformation操作主要包含的算子有：`map()、filter()、select()、aggregate()、groupBy()`。Action操作主要包含的算子有`count()、show()、或者写数据到文件系统`。值得注意的是，Datasets是"惰性的"，代码运行到Transformation类算子并不会马上执行，而是先构建DAG数据流图，当代码运行到Action类算子时才会触发真正的计算


[Spark入门：RDD的设计与运行原理(Python版)](http://dblab.xmu.edu.cn/blog/1681-2/)
[Spark2.1.0+入门：RDD编程(Python版)](http://dblab.xmu.edu.cn/blog/1700-2/)
https://spark.apache.org/docs/2.0.0/api/java/org/apache/spark/sql/Dataset.html
https://spark.apache.org/docs/latest/rdd-programming-guide.html
