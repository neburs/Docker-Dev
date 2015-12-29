FROM nginx:latest
MAINTAINER Ruben "ruben@vasallo.nom.es"

# Add nginx conf
ADD cert/xip.io.crt /etc/ssl/certs/xip.io/xip.io.crt
ADD cert/xip.io.key /etc/ssl/certs/xip.io/xip.io.key 
ADD api.sandbox /etc/nginx/conf.d/api.sandbox.conf

EXPOSE 80 443
CMD ["nginx"]
