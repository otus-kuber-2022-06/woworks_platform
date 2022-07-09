FROM nginx:latest
RUN useradd -r -u 1001 -g nginx appuser

COPY default.conf /etc/nginx/conf.d/default.conf

RUN mkdir /app &&  \
        chown -R appuser:nginx /app && chmod -R 755 /app && \
        chown -R appuser:nginx /var/cache/nginx && \
        chown -R appuser:nginx /var/log/nginx && \
        chown -R appuser:nginx /etc/nginx/conf.d
RUN touch /var/run/nginx.pid && \
        chown -R appuser:nginx /var/run/nginx.pid


#RUN mkdir "/app"
COPY homework.html /app
COPY index.html /app
WORKDIR /app

USER appuser
CMD ["nginx", "-g", "daemon off;"]