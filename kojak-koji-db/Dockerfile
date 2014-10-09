FROM kojak/kojak-common
MAINTAINER John Casey <jcasey@redhat.com>

RUN yum -y install postgresql-server

ENV PGDATA /var/lib/postgresql/data
ENV PGPORT 5432

ADD postgresql-setup /usr/bin/postgresql-setup
RUN chmod +x /usr/bin/postgresql-setup
RUN mkdir -p /var/lib/postgresql
RUN /usr/bin/postgresql-setup initdb

# Borrowed from https://registry.hub.docker.com/u/orchardup/postgresql/dockerfile/
ADD postgresql.conf /var/lib/postgresql/data/postgresql.conf
ADD pg_hba.conf /var/lib/postgresql/data/pg_hba.conf
RUN chown postgres:postgres /var/lib/postgresql/data/*.conf

ADD run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run

EXPOSE 5432

CMD ["/bin/su", "-c", "/usr/local/bin/run", "postgres"]