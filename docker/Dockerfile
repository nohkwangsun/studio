FROM centos:7

# set basic environment
RUN yum install -y git java-1.8.0-openjdk-devel bzip2 \
 && yum groupinstall -y "Development Tools" \
 && curl -s http://apache.mirror.cdnetworks.com/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz | tar xzv \
 && curl -s https://nodejs.org/download/release/v8.11.2/node-v8.11.2-linux-x64.tar.gz | tar xzv \
 && curl -LO https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && sh Miniconda3-latest-Linux-x86_64.sh -b -p /opt/miniconda3 \
 && rm Miniconda3-latest-Linux-x86_64.sh \
 && yum clean all

#ENV PYTHON_HOME=/opt/miniconda3 \
#    JAVA_HOME=/usr/lib/jvm/java \
#    NODEJS_HOME=/opt/node-v8.11.2-linux-x64 \
#    M2_HOME=/opt/apache-maven-3.6.3 \
#
#ENV PATH=$PYTHON_HOME/bin:$JAVA_HOME/bin:$NODEJS_HOME/bin:${M2_HOME}/bin:$PATH


# clone # package # setup
RUN mkdir /git \
 && cd /git \
 && git clone https://github.com/brightics/studio.git \
 && cd /git/studio \
 && mvn clean package -DskipTests \
 && mv /git/studio/build/target/dist/brightics-studio / \
 && rm -rf /git \
 && cd /brightics-studio \
 && sed -i "s/\"127.0.0.1\",/\"0.0.0.0\",/g" visual-analytics/conf.json \
 && sed -i "s/\%\*\ //g" setup.sh \
 && ./setup.sh


# expose the port for visual-analytics
EXPOSE 3000

WORKDIR /brightics-studio
ADD entrypoint.sh /brightics-studio/entrypoint.sh
ENTRYPOINT ["bash", "/brightics-studio/entrypoint.sh"]
