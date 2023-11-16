FROM mysql:8.0.33
LABEL "Project"="Java_Web_App"
LABEL "Author"="Rohit"

ENV MYSQL_ROOT_PASSWORD="rohit"
ENV MYSQL_DATABASE="accounts"

ADD db_backup.sql docker-entrypoint-initdb.d/db_backup.sql
