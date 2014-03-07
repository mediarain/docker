FROM ubuntu

ADD http://nginx.org/keys/nginx_signing.key /usr/local/nginx_signing.key

RUN apt-key add /usr/local/nginx_signing.key
RUN echo "deb http://mirrors.xmission.com/ubuntu precise nginx" > /etc/apt/sources.list
RUN echo "deb http://nginx.org/packages/ubuntu precise nginx" > /etc/apt/sources.list.d/nginx.list
RUN apt-get update
RUN apt-get install -y nginx

EXPOSE 22 80

ENTRYPOINT /usr/sbin/sshd -D
