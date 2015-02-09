FROM phusion/baseimage:0.9.16
MAINTAINER Steve

# Set correct environment variables.
ENV HOME /root

RUN apt-get -y update
RUN apt-get -qy install git python-apt apt-utils python-pip python-dev
RUN pip install PyYAML jinja2 paramiko
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd 
ADD . /src

# Install ansible
RUN pip install ansible==1.7

RUN mkdir -p /etc/ansible/
RUN echo "[local]" > /etc/ansible/hosts
RUN echo "localhost ansible_connection=local" >> /etc/ansible/hosts
WORKDIR /src
RUN ansible-playbook start.yml -c local

#Adding Deamons to containers
RUN mkdir -p /etc/service/rserver
RUN mv /src/runit_scripts/rserver.sh /etc/service/rserver/run
RUN chmod +x /etc/service/rserver/run

RUN mkdir -p /etc/service/mysql
RUN mv /src/runit_scripts/mysql.sh /etc/service/mysql/run
RUN chmod +x /etc/service/mysql/run

RUN mkdir -p /etc/service/apache2
RUN mv /src/runit_scripts/apache2.sh /etc/service/apache2/run
RUN chmod +x /etc/service/apache2/run

RUN mkdir -p /etc/service/shellinabox
RUN mv /src/runit_scripts/shellinabox.sh /etc/service/shellinabox/run
RUN chmod +x /etc/service/shellinabox/run

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

EXPOSE 22 8787 80 4200