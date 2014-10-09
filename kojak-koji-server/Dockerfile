FROM kojak/kojak-common
MAINTAINER John Casey <jcasey@redhat.com>

RUN yum -y install http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm
RUN yum -y update

RUN yum -y install httpd mod_ssl openssl mod_auth_kerb mod_wsgi \
yum-utils rpm-build mock createrepo git cvs svn subversion subversion-tools make rpmbuild pigz \
libvirt virt-install qemu-img screen vim tree links dos2unix mlocate htop iftop \
postgresql-python pyOpenSSL python-krbV python-cheetah python-qpid libvirt-python python-virtinst \
avahi

ADD hostname /etc/hostname

ADD kick-supervisord /usr/local/bin/kick-supervisord
RUN chmod +x /usr/local/bin/kick-supervisord

ADD build-koji /usr/local/bin/build-koji
RUN chmod +x /usr/local/bin/build-koji
RUN /usr/local/bin/build-koji

ADD koji-setup /usr/local/bin/koji-setup
RUN chmod +x /usr/local/bin/koji-setup
RUN /usr/local/bin/koji-setup

ADD avahi-setup /usr/local/bin/avahi-setup
RUN chmod +x /usr/local/bin/avahi-setup
RUN /usr/local/bin/avahi-setup

ADD koji-http.avahi.service /etc/avahi/services/http.service
ADD koji-https.avahi.service /etc/avahi/services/https.service

RUN wget https://raw.github.com/pypa/pip/master/contrib/get-pip.py
RUN python ./get-pip.py
RUN pip install supervisor --pre

ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 80
EXPOSE 443

CMD ["/usr/bin/supervisord"]
