# Forking https://github.com/knqyf263/CVE-2020-8617
FROM centos:7

# CentOS7 EOL was 2024-06-30, so mirrorlist.centos.org is unavailable now.
# Thanks to https://zenn.dev/potyama/articles/83f1d7fc82202e
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-* && \
    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

# Install necessary packages
RUN yum -y update && \
    yum clean all && \
    yum -y groupinstall "Development Tools" && \
    yum install -y epel-release && \
    yum -y install kernel-devel kernel-headers openssl-devel perl-Net-DNS bind-utils vim tar python-pip && \
    pip --trusted-host pypi.python.org --trusted-host files.pythonhosted.org --trusted-host pypi.org install argparse ply

# Install BIND9 from source
COPY bind/src/bind-9.12.4.tar.gz /usr/local/src/bind-9.12.4.tar.gz
RUN cd /usr/local/src && \
    tar zxvf bind-9.12.4.tar.gz && \
    mv bind-9.12.4 bind && \
    rm bind-9.12.4.tar.gz
RUN cd /usr/local/src/bind && \
    ./configure --enable-syscalls --prefix=/var/named/chroot --enable-threads --with-openssl=yes --enable-openssl-version-check --enable-ipv6 --disable-linux-caps && \
    chown -R root:root /usr/local/src/bind && \
    make && \
    make install

# Create device files
RUN mkdir /var/named/chroot/dev && \
    mknod -m 666 /var/named/chroot/dev/null c 1 3 && \
    mknod -m 666 /var/named/chroot/dev/random c 1 8

# Create rndc key
RUN /var/named/chroot/sbin/rndc-confgen -a

RUN mkdir /var/named/chroot/data && \
    mkdir /var/named/chroot/var/log && \
    mkdir /var/named/chroot/var/named

# COPY files
COPY bind/config/named.root /var/named/chroot/var/named/named.root
COPY bind/config/named.conf /var/named/chroot/etc/named.conf
COPY bind/config/named /etc/sysconfig/named
COPY bind/config/example.com.zone /var/named/chroot/var/named/example.com.zone

# Create symbolic link
RUN ln -s /var/named/chroot/etc/rndc.key /etc/rndc.key && \
    ln -s /var/named/chroot/etc/named.conf /etc/named.conf

CMD ["/var/named/chroot/sbin/named", "-g", "-t", "/var/named/chroot", "-c", "/etc/named.conf"]

