FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]
USER root
# Allow buildtime config of HIVE_VERSION
ARG HIVE_VERSION
# Set HIVE_VERSION from arg if provided at build, env if provided at run, or default
ENV HIVE_VERSION=${HIVE_VERSION:-2.3.9}
ENV HIVE_HOME /opt/hive
ENV PATH $HIVE_HOME/bin:$PATH
#install packages
RUN mkdir -p $HIVE_HOME/lib&& \
	cd /opt && \
	wget https://jdbc.postgresql.org/download/postgresql-9.4.1212.jar -O $HIVE_HOME/lib/postgresql-jdbc.jar && \
	apt-get update && apt-get install -y --no-install-recommends openssh-server vim rsync iproute2 inetutils-ping procps  && \
	wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-$HIVE_VERSION/apache-hive-$HIVE_VERSION-bin.tar.gz && \
	tar -xzvf apache-hive-$HIVE_VERSION-bin.tar.gz && \
	mv apache-hive-$HIVE_VERSION-bin hive && \
	rm apache-hive-$HIVE_VERSION-bin.tar.gz && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*
#passwordless ssh
RUN mkdir /var/run/sshd; \
	echo 'root:123456' | chpasswd; \
	sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config; \
	ssh-keygen -q -N "" -t rsa -f /root/.ssh/id_rsa; \
	cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys; \

# SSH login fix. Otherwise user is kicked off after login
	sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

 

ADD jdk-8u77-linux-x64.tar.gz /opt/tools

#hadoop 
ADD hadoop-2.7.2.tar.gz /opt/tools

RUN cd /opt/tools/ && mv hadoop-2.7.2 hadoop

#Hadoop configruation
ADD hadoopconf/*.xml /opt/tools/hadoop/etc/hadoop/
#Hive conf
ADD hiveconf/*.xml $HIVE_HOME/conf/
ADD hiveconf/*.properties $HIVE_HOME/conf/
ADD hiveconf/*.sh $HIVE_HOME/conf/

#set env
RUN sed -i "/export HADOOP_PREFIX/a export JAVA_HOME=/opt/tools/jdk1.8.0_77" /opt/tools/hadoop/libexec/hadoop-config.sh; \
	echo 'export JAVA_HOME=/opt/tools/jdk1.8.0_77' >> /etc/bash.bashrc; \
    echo 'export HADOOP_PREFIX=/opt/tools/hadoop'  >> /etc/bash.bashrc; \
    echo 'export HADOOP_COMMON_HOME=/opt/tools/hadoop'  >> /etc/bash.bashrc; \
    echo 'export HADOOP_HDFS_HOME=/opt/tools/hadoop'  >> /etc/bash.bashrc; \
	echo 'export HADOOP_MAPRED_HOME=/opt/tools/hadoop'  >> /etc/bash.bashrc; \
	echo 'export HADOOP_YARN_HOME=/opt/tools/hadoop'  >> /etc/bash.bashrc; \
	echo 'export HADOOP_CONF_DIR=/opt/tools/hadoop/etc/hadoop'  >> /etc/bash.bashrc; \
	echo 'export YARN_CONF_DIR=$HADOOP_PREFIX/etc/hadoop'  >> /etc/bash.bashrc; \
	echo 'export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_PREFIX/bin:$HADOOP_PREFIX/sbin'  >> /etc/bash.bashrc; \
	echo 'localhost' >> /opt/tools/hadoop/slaves; \
	echo 'master' > /etc/hostname

COPY startup.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/startup.sh

COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh

# Hdfs ports: 50010 50020 50070 50075 50090  Mapred ports: 19888 Yarn ports: 8030 8031 8032 8033 8040 8042 8088
EXPOSE 22 50010 50020 50070 50075 50090 19888 8030 8031 8032 8033 8040 8042 8088 49707 2122 10002 10000
USER $NB_USER

WORKDIR "${HOME}"

ENTRYPOINT ["entrypoint.sh"]
CMD startup.sh