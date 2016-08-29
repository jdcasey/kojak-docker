FROM kojak/kojak-common
MAINTAINER John Casey <jcasey@redhat.com>

RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN yum -y update

RUN yum -y install httpd mod_ssl openssl mod_auth_kerb mod_wsgi \
yum-utils rpm-build mock createrepo git cvs svn subversion subversion-tools make rpmbuild pigz \
libvirt virt-install qemu-img screen vim tree links dos2unix mlocate htop iftop \
postgresql postgresql-python pyOpenSSL python-krbV python-cheetah python-qpid libvirt-python python-virtinst \
avahi

RUN wget https://bootstrap.pypa.io/get-pip.py
RUN python ./get-pip.py
RUN pip install supervisor --pre

ADD conf/supervisord.conf /etc/supervisord.conf

ENV HOST koji
ENV DOMAIN local
ENV GITREPO "git://git.fedorahosted.org/koji"
ENV GITBRANCH "master"

ADD bin/avahi-setup /usr/local/bin/avahi-setup
ADD conf/avahi/ /etc/avahi/services/
RUN /usr/local/bin/avahi-setup

ADD bin/kick-supervisord /usr/local/bin/kick-supervisord
ADD bin/build-koji /usr/local/bin/build-koji

ADD bin/koji-setup /usr/local/bin/koji-setup
ADD bin/run /usr/local/bin/run

RUN chmod +x /usr/local/bin/*

EXPOSE 80
EXPOSE 443

CMD ["/usr/local/bin/run"]
