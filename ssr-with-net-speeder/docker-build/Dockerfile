# ssr-with-net-speeder

FROM centos:7.6.1810
MAINTAINER bestlope <lope365@outlook.com>

RUN yum update -y && \
yum clean all && \
yum install -y python python-pip python-m2crypto gcc-c++ flex bison libnet-devel libpcap-devel git gcc && \
yum clean all

RUN git clone https://github.com/harry-gzcn/ShadowsocksR-1.git ssr
RUN git clone https://github.com/snooda/net-speeder.git net-speeder
WORKDIR net-speeder
RUN sh build.sh

RUN mv net_speeder /usr/local/bin/
COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/net_speeder

# Start Net Speeder
#CMD ["nohup /usr/local/bin/net_speeder venet0 \"ip\" >/dev/null 2>&1 &"]

#Test 
#CMD ["ping www.baidu.com -c 5"]


# Configure container to run as an executable
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
