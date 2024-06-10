## Jupyter containers' base image
FROM jupyter/pyspark-notebook:latest

USER root

# Install python libraries
COPY ./requirements.txt ./
RUN pip install -r requirements.txt

# Image lightweight
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    rsync && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Configure Spark
COPY ./conf/spark-defaults.conf "$SPARK_HOME/conf/spark-defaults.conf"
ENV SPARK_CONF_DIR="$SPARK_HOME/conf"
ENV PYSPARK_PYTHON python3

# Install Jupyter notebook extensions
RUN pip install --upgrade notebook==6.4.12
RUN pip install jupyter_contrib_nbextensions
RUN jupyter contrib nbextension install
RUN jupyter nbextension enable /home/jovyan/work

# Configure AWS (Required - AWS configuration files should be placed under aws/)
RUN mkdir -p /root/.aws
COPY aws /root/.aws

# Expose the default port for Jupyter
EXPOSE 8888
