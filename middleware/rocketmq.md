## RocketMqI学习笔记

1. ### 安装

   ```shell
   wget https://mirror.bit.edu.cn/apache/rocketmq/4.7.1/rocketmq-all-4.7.1-bin-release.zip
   unzip rocketmq-all-4.7.1-source-release.zip
   cd rocketmq-all-4.7.1-bin-release/
   
   nohup sh bin/mqnamesrv &
   tail -f ~/logs/rocketmqlogs/namesrv.log
   
   vim runbroker.sh vim runbroker.sh  调整内存分配参数128m
   # nohup sh bin/mqbroker -n localhost:9876 &
   nohup sh bin/mqbroker -n 192.168.134.136:9876 -c conf/broker.conf autoCreateTopicEnable=true &
   
   sh ./bin/mqbroker -m
   
   tail -f ~/logs/rocketmqlogs/broker.log
   
   export NAMESRV_ADDR=localhost:9876
   sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
   sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
   
   netstat -tlnp
   
   sh bin/mqshutdown broker
   sh bin/mqshutdown namesrv
   
   vim conf/brok
   ```

2. ### 使用