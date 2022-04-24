FROM nginx:1.20.2

WORKDIR /usr/share/nginx/html/

COPY ["index.html", "."]

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]

