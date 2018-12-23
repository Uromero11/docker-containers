FROM nginx:latest
RUN apt-get update
RUN apt-get install -y \
	vim \
	joe \
	inetutils-ping \
	inetutils-traceroute

ADD ./ssl /etc/nginx/ssl
ADD ./nginx.conf /etc/nginx
