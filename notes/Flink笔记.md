## Flink

------

- 初识Flink

  - 什么是Flink：基于有界和无界数据流的有状态的计算，分布式计算引擎
    - 无界数据流：有开始没有结尾，**流处理**实时处理
    - 有界数据流：有开始有结尾，用**批处理**处理有界数据流
  - Flink提供的三个层级的API：
    - SQL/Table API(使用sql语句)
    - DataStream API
    - ProcessFunction
  - Flink运行模式：YARN、Mesos、K8S
  - Flink、Spark Streaming、Storm：**Spark Streaming**以批处理为主，流式处理是批处理的一个特例(将流数据拆成mini batch进行计算)，微批处理框架；**Flink**：以流式为主，批处理是流式处理的一个特例；**Storm**：流式处理，仅支持流
  - 使用场景：事件驱动应用、数据分析应用、数据管道应用

- DataSet API编程(批处理)

- DataStream API编程(流处理)

  - 流处理：实时地处理一些实时数据、实时地产生数据的合集
  - Flink应用程序开发流程（编程模型）：
    - set up execution environment
    - read
    - transform operations
    - sink
    - execute
  - Word Count：

  ```java
  public static void main(String[] agrs) throws Exception{
    // 获取环境
    StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
    // read
  	DataStreamSource<String> data = env.socketTextStream("localhost", 9999);
    // transfer
    dataStreamSource.flatMap(new FlatMapFunction<String, Tuple2<String, Integer>>() {
              @Override
              public void flatMap(String value, Collector<Tuple2<String, Integer>> out) throws Exception {
                  String[] tokens = value.split(",");
                  for (String token : tokens) {
                      if (token.length() > 0) {
                          out.collect(new Tuple2<>(token, 1));
                      }
                  }
              }
          }).keyBy(0).timeWindow(Time.seconds(5)).sum(1).print().setParallelism(1);
    env.execute("job");
  }
  ```

  - 自定义数据源、转换函数(map、filter、split等iterators)、自定义sink(实现SinkFunction)

- Flink Table & SQL

  ```scala
  def main(args: Array[String]): Unit = {
      val env = ExecutionEnvironment.getExecutionEnvironment
      val tableEnv = TableEnvironment.getTableEnvironment(env)
      val path = "file://..."
      val csv = env.readCsvFile[SalesLog](path, ignoreFirstLine = true)
      csv.print()
      val salesTable = tableEnv.fromDataSet(csv)
      tableEnv.registerTable("sales", salesTable)
      val resultTable = tableEnv.sqlQuery("select customerId, sum(amountPaid) money from sales group by customerId")
      tableEnv.toDataSet[Row](resultTable).print()
    }
  case class SalesLog(transactionId: Int, customerId: Int, itemId: Int, amountPaid: Double)
  ```

- Window和Time操作

  - Event Time
  - Ingestion Time
  - Processing Time
  - Window:滑动窗口（数据可能会重叠）、滚动窗口（数据不会重叠）、会话窗口

- Flink Connectors

  - Kafka(source/sink)：FlinkKafkaConsumer和FlinkKafkaProducer
  - ElasticSearch(sink)
  - HDFS(sink)

- Flink部署及作业提交

  - 单机部署：下载Flink源码进行编译：mvn clean install -DskipTests -Dhadoop.version=2.6.1
  - Standalone
  - on YARN

- Flink监控及调优

- water mark:(blog.csdn.net/lmalds/article/details/52704170)

  - window触发的条件：watermark时间 >= window_end_time;在[window_start_time,window_end_time]中有数据存在
  - flink处理乱序：watermark+window机制

water mark、本地跑flink、产品、es