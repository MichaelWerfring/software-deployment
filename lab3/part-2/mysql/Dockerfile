FROM debian:buster

ENV MYSQL_ROOT_PASSWORD='root' \
    MYSQL_DATABASE='wordpress' \
    MYSQL_USER='user' \
    MYSQL_PASSWORD='password' \
    DEBIAN_FRONTEND=noninteractive \
    container=docker

RUN apt-get update && \
    apt-get -y install mariadb-server && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/run/mysqld /var/lib/mysql && \
    echo "mariadb-server mysql-server/root_password password rootpassword" | debconf-set-selections && \
    echo "mariadb-server mysql-server/root_password_again password rootpassword" | debconf-set-selections && \
    sed -i 's/bind-address.*/bind-address = 0.0.0.0/' /etc/mysql/my.cnf && \
    chown -R mysql:mysql /var/lib/mysql /var/run/mysqld && \
    chmod 1777 /var/run/mysqld /var/lib/mysql

EXPOSE 3306/tcp

# Setup DB and User
RUN service mysql start \
    && mysqladmin -u root password $MYSQL_ROOT_PASSWORD \
    && mysql --user=root --password=$MYSQL_ROOT_PASSWORD -e "CREATE DATABASE IF NOT EXISTS ${MYSQL_DATABASE};" \
    && mysql --user=root --password=$MYSQL_ROOT_PASSWORD -e "CREATE USER IF NOT EXISTS '${MYSQL_USER}'@'%' IDENTIFIED BY '${MYSQL_PASSWORD}';" \
    && mysql --user=root --password=$MYSQL_ROOT_PASSWORD -e "GRANT ALL PRIVILEGES ON ${MYSQL_DATABASE}.* TO '${MYSQL_USER}'@'%' IDENTIFIED BY '${MYSQL_PASSWORD}';" \
    && mysql --user=root --password=$MYSQL_ROOT_PASSWORD -e "FLUSH PRIVILEGES;"

# Provide for Wordpress
RUN echo 'bind-address = 0.0.0.0' >> /etc/mysql/my.cnf

VOLUME ["/var/lib/mysql/"]

CMD ["mysqld"]