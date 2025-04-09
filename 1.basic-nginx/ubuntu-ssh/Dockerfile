FROM ubuntu:22.04

RUN apt update && \
    apt install -y openssh-server && \
    echo 'root:rootpass' | chpasswd && \
    mkdir /run/sshd

COPY ./ssh-config/shhd_config ./etc/ssh/sshd_config

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]