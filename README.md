# flink-clickhouse-examples 实战

本例子用flink 1.12.0版本，scala 2.11版本，clickhouse 20.11.4.13版本，kafka 2.6.0版本测试通过  
截止到2021年1月13日，以上版本都是各自的最新版本  
实现kafka接收数据，存储到clickhouse   
  
参考资料：
https://help.aliyun.com/document_detail/185696.html  
https://www.cnblogs.com/qiu-hua/p/13871460.html  
https://www.jianshu.com/p/267148947a35  

## 碰到问题

资料上flink版本分别是10和11，目前最新是12，分别碰到jdbc包不存在  
```xml
<dependency>
    <groupId>org.apache.flink</groupId>
    <artifactId>flink-jdbc_${scala.binary.version}</artifactId>
    <version>${flink.version}</version>
</dependency>
```
和registerTableSource这个方法不存在问题  

【ok】 不出现数据，超过60秒程序退出  
kafka服务出问题，可以以下语句测试，重启kafka即可  
```bash
./bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```

## 测试过程

```sql
mysql --protocol tcp -h host1 -udefault -P 9004 -pGhTY1OeM

CREATE DATABASE IF NOT EXISTS tutorial

CREATE TABLE sink_table (
  id String COMMENT 'id',
  temperature Float64 COMMENT '温度'
  )ENGINE=Log;

./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic sensor
sensor_1,1547718199,35.8
sensor_6,1547718201,15.4
sensor_7,1547718202,6.7
sensor_10,1547718205,38.1
sensor_1,1547718206,32
sensor_1,1547718208,36.2
sensor_1,1547718210,29.7
sensor_1,1547718213,30.9

select * from sink_table
```
  
