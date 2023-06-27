FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:scipy-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

WORKDIR "${HOME}"

EXPOSE 4040

USER root

# Spark dependencies
# Default values can be overridden at build time
ARG spark_version="3.4.0"
ARG hadoop_version="3"
ARG spark_checksum="67bc912e9192ef2159540cb480820e5466dfd91e907c97c5a4787587e3020be042b76c40c51854f2a5dbeb8c3775fe12d9021c1200c4704463ec644132243a69"
# Configure Spark
ENV SPARK_HOME=/usr/local/spark
ENV SPARK_OPTS="--driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info" \
    PATH="${PATH}:${SPARK_HOME}/bin" \
    APACHE_SPARK_VERSION="${spark_version}" \
    HADOOP_VERSION="${hadoop_version}"

RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends "openjdk-17-jre-headless" ca-certificates-java && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Spark installation
WORKDIR /tmp

# You need to use https://archive.apache.org/dist/ website if you want to download old Spark versions
# But it seems to be slower, that's why we use recommended site for download
RUN wget -qO "spark.tgz" "https://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-${APACHE_SPARK_VERSION}/spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz"; \
  echo "${spark_checksum} *spark.tgz" | sha512sum -c - && \
  tar xzf "spark.tgz" -C /usr/local --owner root --group root --no-same-owner && \
  rm "spark.tgz"

RUN ln -s "spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}" "${SPARK_HOME}"; \
  # Add a link in the before_notebook hook in order to source automatically PYTHONPATH && \
  mkdir -p /usr/local/bin/before-notebook.d && \
  ln -s "${SPARK_HOME}/sbin/spark-config.sh" /usr/local/bin/before-notebook.d/spark-config.sh

# Configure IPython system-wide
COPY ipython_kernel_config.py "/etc/ipython/"
RUN fix-permissions "/etc/ipython/"

USER ${NB_UID}

# Install pyarrow+findspark
RUN mamba install --yes 'pyarrow' 'findspark' && \
    mamba clean --all -f -y 

WORKDIR "${HOME}"

