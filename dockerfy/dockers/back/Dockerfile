FROM ubuntu:20.04
LABEL maintainer="ravada@telecos.upc.edu"
LABEL description="Ravada Backend + KVM"

RUN apt-get update \
 && DEBIAN_FRONTEND="noninteractive" TZ=$TZ apt-get install -y -q --no-install-recommends \
    perl libmojolicious-perl libauthen-passphrase-perl \
    libdbd-mysql-perl libdbi-perl libdbix-connector-perl libipc-run3-perl libnet-ldap-perl \
    libproc-pid-file-perl libsys-virt-perl libxml-libxml-perl libconfig-yaml-perl \
    libmoose-perl libjson-xs-perl perlmagick libmoosex-types-netaddr-ip-perl libsys-statistics-linux-perl \
    libio-interface-perl libnet-dns-perl liblocale-maketext-lexicon-perl \
    libmojolicious-plugin-i18n-perl libdbd-sqlite3-perl debconf adduser libdigest-sha-perl libnet-ssh2-perl \
    libfile-rsync-perl libdate-calc-perl libparallel-forkmanager-perl libdatetime-perl libencode-locale-perl netcat-openbsd \
    liblwp-useragent-determined-perl libvirt-clients supervisor net-tools openssh-client apt-utils curl libpbkdf2-tiny-perl \
    libio-stringy-perl libvirt-daemon-system libvirt-clients netcat-openbsd qemu-kvm qemu-utils iproute2 wget bridge-utils firewalld dnsmasq iptables ebtables iptstate \
    libnet-openssh-perl libdatetime-format-dateparse-perl file tzdata libxml2-utils libfile-tee-perl ovmf librrds-perl\
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN echo "listen_tls = 0" >> /etc/libvirt/libvirtd.conf \
 && echo 'listen_tcp = 1' >> /etc/libvirt/libvirtd.conf \
#  && mkdir -p /root/.ssh \
#  && echo "Host ravada-kvm StrictHostKeyChecking no" >/root/.ssh/config \
#  && chmod 600 /root/.ssh/config \
 && mkdir -p /var/log/supervisor \
 && mkdir -p /run/sshd

COPY network.sh default.xml /

EXPOSE 5900-5938
EXPOSE 5940-5950
EXPOSE 55900-55950

COPY supervisord.conf /etc/supervisord.conf

#RUN mkdir /ravada
#ADD src/ravada /ravada
COPY ravada.conf /etc/ravada.conf
WORKDIR /ravada
ENV PERL5LIB /ravada/lib
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
