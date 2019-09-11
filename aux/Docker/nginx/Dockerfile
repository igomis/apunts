# Instalación de Nginx en Ubuntu 16.04
FROM ubuntu:16.04
 
LABEL MAINTAINER="Ignasi Gomis"
 
RUN apt-get update \
  && apt-get install -y nginx \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && ln -sf /dev/stdout /var/log/nginx/access.log \
  && ln -sf /dev/stderr /var/log/nginx/error.log \
  && echo "daemon off;" >> /etc/nginx/nginx.conf
 
COPY default /etc/nginx/sites-available/default
 
EXPOSE 80
CMD ["nginx"]
