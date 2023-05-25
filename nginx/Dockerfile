FROM nginx:alpine

WORKDIR /app

RUN apk add openssl && mkdir -p /var/etc/ssl
RUN openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes \
  -keyout /var/etc/ssl/selfsigned.key -out /var/etc/ssl/selfsigned.crt -subj "/CN=localhost" \
  -addext "subjectAltName=DNS:localhost,IP:127.0.0.1"
RUN curl https://ssl-config.mozilla.org/ffdhe2048.txt > /var/etc/ssl/dhparam

COPY nginx.conf /etc/nginx/conf.d/default.conf
