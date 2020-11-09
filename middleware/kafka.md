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

