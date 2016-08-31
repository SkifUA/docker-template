FROM nginx
#RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
COPY ./default.conf /etc/nginx/conf.d/default.conf

CMD ["nginx", "-g", "daemon off;"]
#CMD ["nginx"]