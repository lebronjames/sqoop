Sqoop1.4.6��װ������ԣ�ʹ��sqoop�������hive�У�ʹ��sqoop����������hive��


һ������sqoop����ѹ������/usr/local
wget https://mirrors.tuna.tsinghua.edu.cn/apache/sqoop/1.4.6/sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
tar -zxvf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
mv sqoop-1.4.6.bin__hadoop-2.0.4-alpha sqoop
mv sqoop /usr/local

�������û�������
�޸�/etc/profile�����ļ�
export  SQOOP_HOME=/opt/sqoop/sqoop-1.4.6.bin__hadoop-2.0.4-alpha
export  PATH=.:${JAVA_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin: ${HIVE_HOME}/bin:${SQOOP_HOME}/bin:$PATH

�����༭sqoop-env.sh�ļ�
���뵽/opt/sqoop/sqoop-1.4.6.bin__hadoop-2.0.4-alpha/confĿ¼�£�Ҳ����ִ�����
cd     /opt/sqoop/sqoop-1.4.6.bin__hadoop-2.0.4-alpha/conf

��sqoop-env-template.sh����һ�ݣ���ȡ��Ϊsqoop-env.sh��Ҳ����ִ�����
cp    sqoop-env-template.sh  sqoop-env.sh

�༭����½���sqoop-env.sh�ļ����༭�����кܶ࣬�������ص����ر༭��Ҳ����ʹ��vim����༭��
       �ڸ��ļ�ĩβ������������ã�
export  HADOOP_COMMON_HOME=/usr/local/hadoop
export  HADOOP_MAPRED_HOME=/usr/local/hadoop
export  HIVE_HOME=/usr/local/hive

�ġ���MySQL���������ص�Sqoop��lib��
cd /usr/local/hive/lib
cp ./mysql-connector-java-5.1.41.jar /usr/local/sqoop/lib/


�塢Mysql�������ݿ�sqoop

����ʹ��Sqoop�鿴MySQL�е����ݱ�
sqoop list-tables --username root --password 'root' --connect  jdbc:mysql://10.5.2.241:3306/sqoop?characterEncoding=UTF-8&useSSL=true

�ߡ�����hive����database��db_hive_people
cd /usr/local/hive
hive
show databases;
create database db_hive_people;

�ˡ�ʹ��sqoop��hive�д�����Ӧ��
sqoop create-hive-table --connect jdbc:mysql://10.5.2.241:3306/sqoop --table people --username root --password root --hive-database  db_hive_people
��ִ���꣬������Hive import complete.������ʾִ�гɹ�

hive�鿴�������
cd /usr/local/hive
hive
show databases;
use db_hive_people;
show tables;
desc people;

�š�ʹ��sqoop����������hive��
����֮ǰִ��create-hive-table����ִ��sqoop import�����ɹ���
sqoop import --connect jdbc:mysql://10.5.2.241:3306/sqoop --table people --username root  -password root --fields-terminated-by ',' --hive-import --hive-database db_hive_people -m 1
��ִ���꣬������Hive import complete.������ʾִ�гɹ���û����ʵ�ʲ��ɹ�

������mysql�����±�tb_student��ûִ��create-hive-table��ֱ��ִ��sqoop import�ɹ���
sqoop import --connect jdbc:mysql://10.5.2.241:3306/sqoop --table tb_student --username root  -password root --fields-terminated-by ',' --hive-import --hive-database db_hive_people -m 1
��ִ���꣬������Hive import complete.������ʾִ�гɹ���û����ʵ�ʳɹ���





