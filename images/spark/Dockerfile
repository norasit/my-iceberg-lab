# lasagna/images/spark/Dockerfile
FROM openjdk:17-slim

SHELL ["/bin/bash", "-c"]

RUN apt-get update && apt-get install -y curl

ENV JAVA_HOME=/usr/local/openjdk-17
ENV PATH="${JAVA_HOME}/bin:$PATH"

#+------------------------+
#| Instalando Spark 3.5.6 |
#+------------------------+

RUN curl -O https://archive.apache.org/dist/spark/spark-3.5.6/spark-3.5.6-bin-hadoop3.tgz && \
    tar zxvf spark-3.5.6-bin-hadoop3.tgz -C /usr/local && \
    rm spark-3.5.6-bin-hadoop3.tgz && \
    cd /usr/local && \
    ln -sT spark-3.5.6-bin-hadoop3 spark

ENV SPARK_HOME=/usr/local/spark
ENV PATH="${SPARK_HOME}/bin:$PATH"

#+------------------------+
#| COPY spark-defaults.conf |
#+------------------------+

COPY spark-defaults.conf ${SPARK_HOME}/conf/

#+----------------------------------+
#| Instalando dependências S3/MinIO |
#+----------------------------------+

RUN curl https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.4/hadoop-aws-3.3.4.jar \
    --output ${SPARK_HOME}/jars/hadoop-aws-3.3.4.jar

RUN curl https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.262/aws-java-sdk-bundle-1.12.262.jar \
    --output ${SPARK_HOME}/jars/aws-java-sdk-bundle-1.12.262.jar

RUN curl https://jdbc.postgresql.org/download/postgresql-42.5.0.jar \
    --output ${SPARK_HOME}/jars/postgresql-42.5.0.jar

#+---------------------------------------------+
#| Instalando dependências para Apache Iceberg |
#+---------------------------------------------+

RUN curl -L https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-spark-runtime-3.5_2.12/1.5.0/iceberg-spark-runtime-3.5_2.12-1.5.0.jar \
    --output ${SPARK_HOME}/jars/iceberg-spark-runtime-3.5_2.12-1.5.0.jar

USER root
