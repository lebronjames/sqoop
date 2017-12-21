Sqoop1.4.6安装部署测试，使用sqoop导入表至hive中，使用sqoop导入数据至hive中


一、下载sqoop，解压，移至/usr/local
wget https://mirrors.tuna.tsinghua.edu.cn/apache/sqoop/1.4.6/sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
tar -zxvf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
mv sqoop-1.4.6.bin__hadoop-2.0.4-alpha sqoop
mv sqoop /usr/local

二、配置环境变量
修改/etc/profile配置文件
export  SQOOP_HOME=/opt/sqoop/sqoop-1.4.6.bin__hadoop-2.0.4-alpha
export  PATH=.:${JAVA_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin: ${HIVE_HOME}/bin:${SQOOP_HOME}/bin:$PATH

三、编辑sqoop-env.sh文件
进入到/opt/sqoop/sqoop-1.4.6.bin__hadoop-2.0.4-alpha/conf目录下，也就是执行命令：
cd     /opt/sqoop/sqoop-1.4.6.bin__hadoop-2.0.4-alpha/conf

将sqoop-env-template.sh复制一份，并取名为sqoop-env.sh，也就是执行命令：
cp    sqoop-env-template.sh  sqoop-env.sh

编辑这个新建的sqoop-env.sh文件，编辑方法有很多，可以下载到本地编辑，也可是使用vim命令编辑。
       在该文件末尾加入下面的配置：
export  HADOOP_COMMON_HOME=/usr/local/hadoop
export  HADOOP_MAPRED_HOME=/usr/local/hadoop
export  HIVE_HOME=/usr/local/hive

四、将MySQL驱动包上载到Sqoop的lib下
cd /usr/local/hive/lib
cp ./mysql-connector-java-5.1.41.jar /usr/local/sqoop/lib/


五、Mysql创建数据库sqoop

六、使用Sqoop查看MySQL中的数据表
sqoop list-tables --username root --password 'root' --connect  jdbc:mysql://10.5.2.241:3306/sqoop?characterEncoding=UTF-8&useSSL=true

七、现在hive创建database：db_hive_people
cd /usr/local/hive
hive
show databases;
create database db_hive_people;

八、使用sqoop在hive中创建相应表
sqoop create-hive-table --connect jdbc:mysql://10.5.2.241:3306/sqoop --table people --username root --password root --hive-database  db_hive_people
当执行完，看到“Hive import complete.”，表示执行成功

hive查看创建结果
cd /usr/local/hive
hive
show databases;
use db_hive_people;
show tables;
desc people;

九、使用sqoop导入数据至hive中
由于之前执行create-hive-table，再执行sqoop import，不成功。
sqoop import --connect jdbc:mysql://10.5.2.241:3306/sqoop --table people --username root  -password root --fields-terminated-by ',' --hive-import --hive-database db_hive_people -m 1
当执行完，看到“Hive import complete.”，表示执行成功。没报错，实际不成功

重新在mysql创建新表tb_student，没执行create-hive-table，直接执行sqoop import成功。
sqoop import --connect jdbc:mysql://10.5.2.241:3306/sqoop --table tb_student --username root  -password root --fields-terminated-by ',' --hive-import --hive-database db_hive_people -m 1
当执行完，看到“Hive import complete.”，表示执行成功。没报错，实际成功。





