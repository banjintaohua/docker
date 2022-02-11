FROM centos:7

RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup \
    && curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo \
    && yum clean all \
    && yum makecache fast \
    && yum install -y vim openssh \
    && sed -ie 's/keepcache=0/keepcache=1/g' /etc/yum.conf

CMD tail -f /dev/null