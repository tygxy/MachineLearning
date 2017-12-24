# Hadoop相关生态圈技术

## 0.Hadoop的安装
- 安装jdk 
- 设置ip和hostname的映射关系,ssh等
- 配置文件
    - 	hadoop-env.sh 
    ```
        export JAVA_HOME=/home/hadoop/app/jdk1.7.0_51
    ```
    - core-site.xml
    ```
         <property>
                <name>fs.defaultFS</name>
                <value>hdfs://hadoop001:8020</value>
         </property>	
    	    <property>
                <name>hadoop.tmp.dir</name>
                <value>/home/hadoop/app/tmp</value>
    	    </property>	
    ```
    - hdfs-site.xml
    ```
         <property>
             <name>dfs.replication</name>
             <value>1</value>
         </property>
    ```
- yarn
    - yarn-site.xml
    ```
    	<property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
         </property>
    ```
    - mapred-site.xml
    ```
    	<property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
    ```

## 1.HDFS
- 块，默认128MB，抽象概念，操作都是以块为基本单元，默认备份数为三，NameNode管理文件系统命名空间，维护文件目录树和索引目录，DataNode具体存储文件
- 副本存放策略是本地机架节点，同一机架节点，不同机架节点
- 如何保证NameNode的安全，会备份NameNode持久化的元数据文件到其他文件系统中，系统同步运行SecondNameNode,周期性合并编辑日志中的命名镜像
- 文件的读取
  - DistributedFileSystem会通过RPC协议调用NameNode确定请求文件块所在位置，返回DataNode
  - 返回的DataNode会按照机器拓扑结构得出与客户端的距离，进行排序，读取最近DataNode的数据
- 文件写入
  - 以流的形式传给不同的DataNode节点，并且需要返回确认结果




## 2.《以慕课网日志分析为例 进入大数据Spark SQL的世界》
### 2.1 初探大数据
- hadoop包括HDFS，yarn(资源调度)，mapreduce
- hdfs 
   - NN：
  1）负责客户端请求的响应
  2）负责元数据（文件的名称、副本系数、Block存放的DN）的管理
   - DN：
  1）存储用户的文件对应的数据块(Block)
  2）要定期向NN发送心跳信息，汇报本身及其所有的block信息，健康状况
- yarn
   - ResourceManager的职责：一个集群active状态的RM只有一个，负责整个集群的资源管理和调度
     1）处理客户端的请求(启动/杀死)
     2）启动/监控ApplicationMaster(一个作业对应一个AM)
     3）监控NM
     4）系统的资源分配和调度
   - NodeManager：整个集群中有N个，负责单个节点的资源管理和使用以及task的运行情况
        1）定期向RM汇报本节点的资源使用请求和各个Container的运行状态
        2）接收并处理RM的container启停的各种命令
        3）单个节点的资源管理和任务管理
   - ApplicationMaster：每个应用/作业对应一个，负责应用程序的管理
        1）数据切分
        2）为应用程序向RM申请资源(container)，并分配给内部任务
        3）与NM通信以启停task， task是运行在container中的
        4）task的监控和容错
   - YARN执行流程
        1）用户向YARN提交作业
        2）RM为该作业分配第一个container(AM)
        3）RM会与对应的NM通信，要求NM在这个container上启动应用程序的AM
        4) AM首先向RM注册，然后AM将为各个任务申请资源，并监控运行情况
        5）AM采用轮训的方式通过RPC协议向RM申请和领取资源
        6）AM申请到资源以后，便和相应的NM通信，要求NM启动任务
        7）NM启动我们作业对应的task
       
### 2.2 Spark及其生态圈概述
- MapReduce的局限性：
        1）代码繁琐；
        2）只能够支持map和reduce方法；
        3）执行效率低下，中间数据要落磁盘；
        4）不适合迭代多次、交互式、流式的处理；
        
### 2.5 Hive过渡到Spark SQL
- 
