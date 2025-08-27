FROM mysql:8

ENV MYSQL_ROOT_PASSWORD=root
ENV MYSQL_DATABASE=atividade02
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=password

COPY codigo.sql /docker-entrypoint-initdb.d/
