# images/hive-metastore/Dockerfile

FROM openjdk:17-slim-bullseye

SHELL ["/bin/bash", "-c"]

WORKDIR /opt

# Fix APT source issue
RUN apt-get update && apt-get install -y curl

ENV HADOOP_HOME=/opt/hadoop-3.3.4
ENV HIVE_HOME=/opt/apache-hive-metastore-3.0.0-bin

# Install Hive Standalone Metastore
RUN curl -O https://archive.apache.org/dist/hive/hive-standalone-metastore-3.0.0/hive-standalone-metastore-3.0.0-bin.tar.gz && \
    tar xfz hive-standalone-metastore-3.0.0-bin.tar.gz && \
    rm hive-standalone-metastore-3.0.0-bin.tar.gz

# Install Hadoop Common
RUN curl -O https://archive.apache.org/dist/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz && \
    tar zxf hadoop-3.3.4.tar.gz && \
    rm hadoop-3.3.4.tar.gz

# Install JDBC connector for Postgres
RUN curl -o ${HIVE_HOME}/lib/postgresql-42.5.0.jar \
    https://jdbc.postgresql.org/download/postgresql-42.5.0.jar

# Add MinIO/AWS dependencies
RUN curl --output ${HADOOP_HOME}/share/hadoop/tools/lib/hadoop-aws-3.3.4.jar \
    https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.4/hadoop-aws-3.3.4.jar

RUN curl --output ${HADOOP_HOME}/share/hadoop/tools/lib/aws-java-sdk-bundle-1.12.262.jar \
    https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.262/aws-java-sdk-bundle-1.12.262.jar

ENV HADOOP_CLASSPATH=${HADOOP_HOME}/share/hadoop/tools/lib/aws-java-sdk-bundle-1.12.262.jar:${HADOOP_HOME}/share/hadoop/tools/lib/hadoop-aws-3.3.4.jar

# Copy Hive config and entry script
COPY conf/metastore-site.xml ${HIVE_HOME}/conf/
COPY conf/entrypoint.sh /usr/entrypoint.sh

EXPOSE 9083

ENTRYPOINT ["sh", "/usr/entrypoint.sh"]
