# 基础镜像
FROM harbor.pharmaoryx.net/official/centos:7.9.2009

# 维护者
MAINTAINER pharmaoryx "harbor.pharmaoryx.net"

# 时区环境变量
ENV TZ "Asia/Shanghai"

# 基础依赖包
# rsync 同步更新两处计算机的文件与目录
# [freetype](https://zh.wikipedia.org/wiki/FreeType): 可以用来将字符栅格化并映射成位图以及提供其他字体相关业务的支持
RUN yum install -y which rsync \  
    && yum install -y freetype-devel freetype \
    && yum install -y libpng libpng-devel libffi-devel libxml2 libxml2-devel libxslt-devel \
    && yum install -y gcc gcc-c++ make \
    && yum install -y libcdio-devel faac-devel ffmpeg-devel.x86_64 ffmpeg.x86_64 faad2 faad2-devel faad2-libs \
    && yum install -y libtiff-devel libjpeg-devel libzip-devel freetype-devel lcms2-devel libwebp-devel tcl-devel tk-devel \
    # 安装 MySQL 的 client & develop(注意尽量使用5.6版本，支持utf8mb4版本)
    && yum install -y openssl-devel cyrus-sasl-devel \
    && yum install -y Percona-Server-devel-56.x86_64 Percona-Server-client-56.x86_64 \
    && yum install -y net-too ls cronie lsof \
    && yum install -y librdkafka-devel librdkafka \
    && yum update -y \
    && yum clean all \
    && useradd -U chunyu
# 注: 不直接安装官方提供的sqlite-devel(3.7.17版本)，而是安装自己编译的3.26版本
# sqlite 3.12.1版本之前有bug.
# https://behind.pretix.eu/2018/06/05/too-many-tests/
# 自己编译sqlite-cy rpm包：https://app.yinxiang.com/shard/s54/nl/11570155/0bffccce-ce0a-44d6-b2b3-92a9a67f3c21/
# 其中`registry.chunyutianxia.com/base/cy_centos_7_2:v2.0`镜像就是上面内容除去sqlite-cy后打的镜像就可以

COPY pip.conf /root/.pip/pip.conf

# 上面yum会重装glibc-common，locale和这个包关联，所以放到下面执行
# 支持中文(en_US.utf8其实也可以), 时区选择
RUN localedef -i zh_CN -c -f UTF-8 zh_CN.UTF-8
ENV LANG=zh_CN.utf8 LANGUAGE=zh_CN.utf8 LC_ALL=zh_CN.utf8 TZ=Asia/Shanghai

ENV LD_LIBRARY_PATH /usr/local/lib64:/usr/local/lib:$LD_LIBRARY_PATH

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

