FROM nginx:latest
ARG COLOR="pink"
RUN apt-get update ; apt-get install curl -y
WORKDIR /usr/share/nginx/html
COPY . /usr/share/nginx/html
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD curl -f http://localhost/ || exit 1
RUN sed -e s/COLOR/"$COLOR"/ myweb.html > index.html ; rm myweb.html
CMD  nginx -g 'daemon off;'