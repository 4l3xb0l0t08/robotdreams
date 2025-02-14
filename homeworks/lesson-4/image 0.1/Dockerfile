FROM ubuntu:latest
RUN apt update
RUN apt install -y nginx
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]
