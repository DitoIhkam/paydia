# Menggunakan image nginx:alpine sebagai base image
FROM nginx:alpine

# Menyalin default.conf ke direktori nginx config jika folder lokal kosong
VOLUME ["/etc/nginx/conf.d"]

# Menyalin file default.conf
COPY ./default.conf /etc/nginx/conf.d/

# Mengatur environment variable
ENV HTTP_PORT=80
ENV REVERSE_HOST=http://info.cern.ch/

# Konfigurasi untuk nginx
RUN echo "server {" > /etc/nginx/conf.d/default.conf && \
    echo "  listen ${HTTP_PORT};" >> /etc/nginx/conf.d/default.conf && \
    echo "  location / {" >> /etc/nginx/conf.d/default.conf && \
    echo "    proxy_pass ${REVERSE_HOST};" >> /etc/nginx/conf.d/default.conf && \
    echo "    proxy_set_header Host \$host;" >> /etc/nginx/conf.d/default.conf && \
    echo "    proxy_set_header X-Real-IP \$remote_addr;" >> /etc/nginx/conf.d/default.conf && \
    echo "    proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;" >> /etc/nginx/conf.d/default.conf && \
    echo "    proxy_set_header X-Forwarded-Proto \$scheme;" >> /etc/nginx/conf.d/default.conf && \
    echo "  }" >> /etc/nginx/conf.d/default.conf && \
    echo "}" >> /etc/nginx/conf.d/default.conf

# Expose port yang digunakan oleh nginx
EXPOSE ${HTTP_PORT}

# Jalankan nginx di background
CMD ["nginx", "-g", "daemon off;"]