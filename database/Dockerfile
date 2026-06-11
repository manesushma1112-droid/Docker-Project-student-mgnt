FROM postgres:13

ENV POSTGRES_USER=admin
ENV POSTGRES_PASSWORD=123456
ENV POSTGRES_DB=mydatabase

COPY init.sql /docker-entrypoint-initdb.d/
