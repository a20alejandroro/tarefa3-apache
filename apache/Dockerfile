FROM debian:12

RUN apt update && apt install -y apache2 && apt clean && rm -rf /var/lib/apt/lists/*

RUN a2enmod auth_digest
RUN a2enmod authz_groupfile
RUN a2enmod ssl
EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"]

