# FIXME: tutum/debian will be deprecated soon: https://github.com/tutumcloud/tutum-debian/blob/master/README.md
#FROM tutum/debian:jessie
#RUN rm /etc/apt/apt.conf.d/docker-clean && apt-get update && apt-get install -y sudo net-tools wget sysvinit-core sysvinit sysvinit-utils curl vim man faketime unzip iptables iputils-ping logrotate && apt-get remove -y --purge --auto-remove systemd

FROM ubuntu:16.04

# Install packages
RUN apt-get update && \
    apt-get -y install openssh-server pwgen && \
    mkdir -p /var/run/sshd && \
    sed -i "s/UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config && \
    sed -i "s/PermitRootLogin without-password/PermitRootLogin yes/g" /etc/ssh/sshd_config && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config

# https://docs.docker.com/engine/examples/running_ssh_service/
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN rm /etc/apt/apt.conf.d/docker-clean && apt-get update && apt-get install -y sudo net-tools wget systemd-sysv sysvinit-utils curl vim man faketime unzip iptables iputils-ping logrotate && apt-get remove -y --purge --auto-remove systemd

ADD set_root_pw.sh /set_root_pw.sh
ADD run.sh /run.sh
RUN chmod +x /*.sh

ENV AUTHORIZED_KEYS **None**

EXPOSE 22

CMD ["/run.sh"]