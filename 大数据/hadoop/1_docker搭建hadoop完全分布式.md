# Docker 搭建hadoop完全分布式

## 准备所需的安装包与所需资源

1. JDK8 安装包`jdk-8u231-linux-x64.tar.gz`
2. Hadoop 安装包`hadoop-2.10.0.tar.gz`
3. 事先配置好的Hadoop配置文件
   - **core-site.xml**

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
    <property>
        <name>fs.defaultFS</name>
            <value>hdfs://namenode1:9000</value>
    </property>
    <property>
            <name>hadoop.tmp.dir</name>
            <value>/usr/local/module/hadoop-2.10.0/data/tmp</value>
    </property>
    <property>
            <name>io.file.buffer.size</name>
            <value>131072</value>
    </property>
    </configuration>
    ```

   - hdfs-site.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
    <property>
            <name>dfs.replication</name>
            <value>3</value>
    </property>
    </configuration>
    ```

   - mapred-site.xml

    ```xml
    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
    <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
    </property>
    </configuration>
    ```

   - yarn-site.xml

    ```xml
    <?xml version="1.0"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->
    <configuration>

    <!-- Site specific YARN configuration properties -->
    <property>
            <name>yarn.resourcemanager.hostname</name>
            <value>namenode1</value>
    </property>
    <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
    </property>

    </configuration>
    ```

## 编写dockerfile文件构建镜像

```shell
# 基础镜像
FROM centos:7
#拷贝所需的安装包
COPY ["jdk-8u231-linux-x64.tar.gz","hadoop-2.10.0.tar.gz","/usr/local/software/"]
#解压缩包，并修改hadoop env中JAVA_HOME
RUN mkdir -p /usr/local/module && \
    tar -zxvf /usr/local/software/jdk-8u231-linux-x64.tar.gz -C /usr/local/module/ && \
    tar -zxvf /usr/local/software/hadoop-2.10.0.tar.gz -C /usr/local/module/ && \
    rm -rf /usr/local/software/jdk-8u231-linux-x64.tar.gz && \
    rm -rf /usr/local/software/hadoop-2.10.0.tar.gz && \
    sed -i "s/export JAVA_HOME=.*/export JAVA_HOME=\/usr\/local\/module\/jdk1.8.0_231/g" /usr/local/module/hadoop-2.10.0/etc/hadoop/hadoop-env.sh && \
    sed -i "s/# export JAVA_HOME=.*/export JAVA_HOME=\/usr\/local\/module\/jdk1.8.0_231/g" /usr/local/module/hadoop-2.10.0/etc/hadoop/mapred-env.sh && \
    sed -i "s/# export JAVA_HOME=.*/export JAVA_HOME=\/usr\/local\/module\/jdk1.8.0_231/g" /usr/local/module/hadoop-2.10.0/etc/hadoop/yarn-env.sh
#复制文件到容器中
COPY ["core-site.xml","hdfs-site.xml","mapred-site.xml","yarn-site.xml","/usr/local/module/hadoop-2.10.0/etc/hadoop/"]
#yum安装所需包
RUN yum update -y && yum install -y lsof
#yum安装所需包
RUN yum install -y vim net-tools scp
#yum安装所需包
RUN yum install -y openssh-server openssh-clients
#修改免密登录配置，重置root密码
RUN sed -i 's/session    required     pam_loginuid.so/#session    required     pam_loginuid.so/' /etc/pam.d/sshd && \
    sed -i 's/UsePAM yes/UsePAM no/' /etc/ssh/sshd_config && \
    echo "root:123456" | chpasswd
#生成密钥
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N "" && \
    ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N "" && \
    ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
#将公共密钥拷贝到/root/.ssh/authorized_keys
RUN ssh-keygen -t rsa -f "/root/.ssh/id_rsa" -N "" && cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
#设置JAVA_HOME
ENV JAVA_HOME /usr/local/module/jdk1.8.0_231
#设置CLASSPATH
ENV CLASSPATH .:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
#设置HADOOP_HOME
ENV HADOOP_HOME /usr/local/module/hadoop-2.10.0
#设置HPATH
ENV PATH ${JAVA_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:$PATH
#映射端口
EXPOSE 22
#设置启动命令
CMD ["/usr/sbin/sshd", "-D"]
```

## 构建镜像

`docker build -t centos7-hadoop:v1 .`

## 启动容器

- `docker run -it -h namenode1 -p 50070:50070 -p 8088:8088 -p 19888:19888 --name namenode1 --privileged centos7-hadoop:v1`
- `docker run -it -h datanode1 --name datanode1 --privileged centos7-hadoop:v1`
- `docker run -it -h datanode2 --name datanode2 --privileged centos7-hadoop:v1`
- `docker run -it -h datanode3 --name datanode3 --privileged centos7-hadoop:v1`

## 进入容器

- `docker exec -it [namenode1-container-id] /bin/bash`
- `docker exec -it [datanode1-container-id] /bin/bash`
- `docker exec -it [datanode2-container-id] /bin/bash`
- `docker exec -it [datanode3-container-id] /bin/bash`

首次进入容器，在各容器中ssh 其他节点，下次ssh即可免密登录。

修改各个容器中hosts文件，位置在`/etc/hosts`，添加如下内容，`xx`表示ip地址

```no
namenode1   xxx.xx.xx.xx
datanode1   xxx.xx.xx.xx
datanode2   xxx.xx.xx.xx
datanode3   xxx.xx.xx.xx
```

## 在namenode1容器中修改hadoop slaves文件

`$HADOOP_HOME/etc/hadoop/slaves`文件替换如下内容

```no
datanode1
datanode2
datanode3
```

## 在namenode1容器启动hadoop服务

1. `hadoop namenode -format`
2. `start-all.sh`
