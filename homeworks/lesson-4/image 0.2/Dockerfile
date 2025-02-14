FROM nginx:alpine3.21
RUN apk update && apk upgrade && rm -rf /var/cache/apk/*
COPY index.html /usr/share/nginx/html/
CMD ["nginx", "-g", "daemon off;"]
