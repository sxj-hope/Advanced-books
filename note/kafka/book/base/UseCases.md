## 使用案例

以下描述了一些 ApacheKafka ®的流行用例。有关这些领域的概述，请参阅 [此博客中的文章](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying/)

#### 消息

Kafka 很好地替代了传统的message broker（消息代理）。 Message brokers 可用于各种场合（如将数据生成器与数据处理解耦，缓冲未处理的消息等）。 与大多数消息系统相比，Kafka拥有更好的吞吐量、内置分区、具有复制和容错的功能，这使它成为一个非常理想的大型消息处理应用。

根据我们的经验，通常消息传递使用较低的吞吐量，但可能要求较低的端到端延迟，Kafka提供强大的持久性来满足这一要求。

在这方面，Kafka 可以与传统的消息传递系统（[ActiveMQ](http://activemq.apache.org/) 和 [RabbitMQ](https://www.rabbitmq.com/) ）相媲美。

#### 跟踪网站活动

Kafka 的初始用例是将用户活动跟踪管道重建为一组实时发布-订阅源。 这意味着网站活动（浏览网页、搜索或其他的用户操作）将被发布到中心topic，其中每个活动类型有一个topic。 这些订阅源提供一系列用例，包括实时处理、实时监视、对加载到Hadoop或离线数据仓库系统的数据进行离线处理和报告等。

每个用户浏览网页时都生成了许多活动信息，因此活动跟踪的数据量通常非常大

#### 度量

Kafka 通常用于监控数据。这涉及到从分布式应用程序中汇总数据，然后生成可操作的集中数据源。

#### 日志聚合

许多人使用Kafka来替代日志聚合解决方案。 日志聚合系统通常从服务器收集物理日志文件，并将其置于一个中心系统（可能是文件服务器或HDFS）进行处理。 Kafka 从这些日志文件中提取信息，并将其抽象为一个更加清晰的消息流。 这样可以实现更低的延迟处理且易于支持多个数据源及分布式数据的消耗。 与Scribe或Flume等以日志为中心的系统相比，Kafka具备同样出色的性能、更强的耐用性（因为复制功能）和更低的端到端延迟。

#### 流处理

许多Kafka用户通过管道来处理数据，有多个阶段： 从Kafka topic中消费原始输入数据，然后聚合，修饰或通过其他方式转化为新的topic， 以供进一步消费或处理。 例如，一个推荐新闻文章的处理管道可以从RSS订阅源抓取文章内容并将其发布到“文章”topic; 然后对这个内容进行标准化或者重复的内容， 并将处理完的文章内容发布到新的topic; 最终它会尝试将这些内容推荐给用户。 这种处理管道基于各个topic创建实时数据流图。从0.10.0.0开始，在Apache Kafka中，[Kafka Streams](https://kafka.apachecn.org/documentation/streams) 可以用来执行上述的数据处理，它是一个轻量但功能强大的流处理库。除Kafka Streams外，可供替代的开源流处理工具还包括[Apache Storm](https://storm.apache.org/) 和[Apache Samza](http://samza.apache.org/).

#### 采集日志

[Event sourcing](http://martinfowler.com/eaaDev/EventSourcing.html) 是一种应用程序设计风格，按时间来记录状态的更改。 Kafka 可以存储非常多的日志数据，为基于 event sourcing 的应用程序提供强有力的支持。

#### 提交日志

Kafka 可以从外部为分布式系统提供日志提交功能。 日志有助于记录节点和行为间的数据，采用重新同步机制可以从失败节点恢复数据。 Kafka的日志压缩 功能支持这一用法。 这一点与[Apache BookKeeper](http://zookeeper.apache.org/bookkeeper/) 项目类似。