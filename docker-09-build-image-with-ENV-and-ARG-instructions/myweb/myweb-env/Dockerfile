FROM nginx:latest
WORKDIR /usr/share/nginx/html  
ENV COLOR="red"
RUN apt-get update ; apt-get install curl -y
COPY . .
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 CMD curl -f http://localhost/ || exit 1
CMD sed -e s/COLOR/"$COLOR"/ myweb.html  > index.html ; rm myweb.html ; nginx -g 'daemon off;'