FROM nginx:latest
VOLUME /tmp
COPY ./web.txt /usr/share/nginx/html/index.html
RUN chmod 755 /usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]
