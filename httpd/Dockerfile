FROM ubuntu:latest

MAINTAINER paritosh.anand@makemytrip.com

RUN apt-get update && apt-get install -y --no-install-recommends apache2 libapache2-mod-jk

ADD apache2.conf /etc/apache2/apache2.conf

# copy /etc/apache2/sites-enabled/000-default.conf
ADD 000-default.conf /etc/apache2/sites-enabled/000-default.conf

# copy /etc/libapache2-mod-jk/workers.properties 
ADD worker.properties /etc/libapache2-mod-jk/workers.properties

ADD jk.conf /etc/apache2/mods-available/jk.conf

VOLUME ["/var/log/apache2"]

EXPOSE 80 443

CMD ["apachectl", "-k", "start", "-DFOREGROUND"]