### 一. install

#### JDK

wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u141-b15/336fa29ff2bb4ef291e347e091f7f4a7/jdk-8u141-linux-x64.tar.gz"

tar zxvf jdk-8u141-linux-x64.tar.gz

vim /etc/profile

export JAVA_HOME=/opt/jdk.1.8.0_141
export JRE_HOME=$JAVA_HOME/jre
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=./:$JAVA_HOME/lib:$JRE_HOME/lib

#### Zookeeper

wget https://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz

tar zxvf apache-zookeeper-3.6.2-bin.tar.gz

#### Kafka

bin/kafka-topics.sh --zookeeper 127.0.0.1:2181/kafka --create --topic topic-demo --replication-factor 1 --partitions 4

bin/kafka-topics.sh --zookeeper 127.0.0.1:2181/kafka --list

bin/kafka-topics.sh --zookeeper 127.0.0.1:2181/kafka --describe --topic topic-demo

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic topic-demo

bin/kafka-server-start.sh config/server.properties

bin/kafka-server-stop.sh

172.17.0.17