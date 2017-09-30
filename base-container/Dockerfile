FROM microsoft/aspnetcore
ENV HOME /home
RUN apt-get update \ 
    && apt-get install -my wget gnupg \ 
    && curl -sL https://deb.nodesource.com/setup_8.x | bash - \
    && apt-get install -y nodejs openssh-server

COPY sshd_config /etc/ssh

EXPOSE 2222
