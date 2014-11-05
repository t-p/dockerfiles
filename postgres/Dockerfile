FROM debian:wheezy

ENV DEBIAN_FRONTEND noninteractive

RUN groupadd -r postgres && useradd -r -g postgres postgres

# grab gosu for easy step-down from root
RUN apt-get update && apt-get install -y curl \
	&& curl -o /usr/local/bin/gosu -SL 'https://github.com/tianon/gosu/releases/download/1.1/gosu' \
	&& chmod +x /usr/local/bin/gosu \
	&& apt-get purge -y --auto-remove curl

# make the "en_US.UTF-8" locale so postgres will be utf-8 enabled by default
RUN apt-get update && apt-get install -y locales \
	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv ACCC4CF8
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/  wheezy-pgdg main" | tee /etc/apt/sources.list.d/pgdg.list

RUN apt-get update
RUN apt-get install -y python-software-properties software-properties-common \
      postgresql-$PG_MAJOR \
      postgresql-contrib-$PG_MAJOR

RUN rm -rf /var/ilb/apt/lists/*

ENV PG_MAJOR 9.3
ENV PATH /usr/lib/postgresql/$PG_MAJOR/bin:$PATH
ENV PGDATA /var/lib/postgresql/data
VOLUME /var/lib/postgresql/data

COPY ./docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 5432
CMD ["postgres"]
