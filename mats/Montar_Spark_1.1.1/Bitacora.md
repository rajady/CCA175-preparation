#################################################################################
#### En AWS sobre EC2
#################################################################################

1º Generar las máquianas, para nosotros han sido 3:

  ->  kqof-env-spark-standalone-00 t2.micro:
          Privada: 192.168.3.213
          Publica: 54.194.217.177
  ->  kqof-env-spark-standalone-01 t2.micro
          Privada: 192.168.3.214
          Publica: 54.154.209.65
  ->  kqof-env-spark-standalone-02 t2.micro
          Privada: 192.168.3.215
          Publica: 54.194.84.90

2º Update system and isntalling tools (git, jdk, sbt):
  
  -> sudo yum update
  -> sudo yum update
  -> sudo yum upgrade
  -> sudo yum install wget
  -> sudo yum install vim
  -> sudo yum install git
  -> sudo yum install bash-completion

    ******************************************************************************************
    ***** JAVA 8
    ******************************************************************************************
    -> java -version (No debe estar instalada)
    -> cd /opt/
    -> sudo wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u111-b14/jdk-8u111-linux-x64.tar.gz"

    -> sudo tar xzf jdk-8u111-linux-x64.tar.gz
    -> cd /opt/jdk1.8.0_111/
    -> sudo alternatives --install /usr/bin/java java /opt/jdk1.8.0_111/bin/java 2
    -> sudo alternatives --config java
    -> sudo alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_111/bin/jar 2
    -> sudo alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_111/bin/javac 2
    -> sudo alternatives --set jar /opt/jdk1.8.0_111/bin/jar
    -> sudo alternatives --set javac /opt/jdk1.8.0_111/bin/javac
    -> java -version
    -> Changes in profile:
            export JAVA_HOME=/opt/jdk1.8.0_111
            export JRE_HOME=/opt/jdk1.8.0_111/jre
            export PATH=$PATH:/opt/jdk1.8.0_111/bin:/opt/jdk1.8.0_111/jre/bin

3º Descargar Spark, nosotros, la versión 1.6.3:
  
  -> cd ~
  -> mkdir materials
  -> cd materials
  -> git clone git://github.com/apache/spark.git
  -> git checkout branch-1.6
  -> ./[SPARK DIR]/sbt/sbt assembly 1> log1.log 2> log2.log &

4º Ejecutar en el nodo master:
  -> ./[SPARK DIR]/sbin/start-master.sh 

5º Ejecutar en los nodos workers:
  -> ./[SPARK DIR]/sbin/start-slave.sh spark://[IP-MASTER-NODE]:7077 




###############################################################
COMMANDS
###############################################################

spark-shell --packages org.apache.spark:spark-streaming-kafka_2.10:1.6.3

sc.setLocalProperty("spark.driver.allowMultipleContexts", "true")

val ssc = new StreamingContext(sc, Seconds(1))

