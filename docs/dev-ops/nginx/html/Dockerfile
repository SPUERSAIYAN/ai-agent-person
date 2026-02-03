FROM registry.cn-hangzhou.aliyuncs.com/xfg-studio/nginx:1.25.1

# 设置工作目录
WORKDIR /usr/share/nginx/html

# 1. 删除 Nginx 默认的静态文件
RUN rm -rf ./*

# 2. 将当前目录下的所有静态资源拷贝到镜像中
# 假设你的 index.html 就在项目根目录
COPY . .

# 3. 覆盖 Nginx 配置（解决静态项目可能需要的配置）
RUN echo 'server {' > /etc/nginx/conf.d/default.conf && \
    echo '    listen 80;' >> /etc/nginx/conf.d/default.conf && \
    echo '    server_name localhost;' >> /etc/nginx/conf.d/default.conf && \
    echo '    root /usr/share/nginx/html;' >> /etc/nginx/conf.d/default.conf && \
    echo '    index index.html;' >> /etc/nginx/conf.d/default.conf && \
    echo '    location / {' >> /etc/nginx/conf.d/default.conf && \
    echo '        try_files $uri $uri/ /index.html;' >> /etc/nginx/conf.d/default.conf && \
    echo '    }' >> /etc/nginx/conf.d/default.conf && \
    echo '}' >> /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]