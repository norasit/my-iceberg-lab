# images/trino/Dockerfile

FROM trinodb/trino

USER root

# Copy catalog configurations
COPY ./conf/hive.properties /etc/trino/catalog/
COPY ./conf/delta.properties /etc/trino/catalog/
COPY ./conf/iceberg.properties /etc/trino/catalog/

# Prepare Hadoop config directories
RUN mkdir -p /etc/hadoop/conf

# Copy Hive Metastore config (for Hadoop compatibility)
COPY ./conf/metastore-site.xml /etc/hadoop/conf/
