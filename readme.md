# Start the compose

```
$ docker-compose up
```
or

```
$ docker-compose up -d
```

# Docker-compose.yml file

```
web:
  image: nginx
  volumes:
   - ./nginx_conf:/etc/nginx/conf.d
  ports:
   - "80:80"
  environment:
   - NGINX_HOST=foobar.com
   - NGINX_PORT=80
```

# Nginx config as mirror

/etc/nginx/conf.d/mirror.conf

```
server  {

        listen 80;

        location / {
                mirror /root_mirror;
                mirror_request_body on;
                proxy_pass http://172.17.0.1:8080;
                proxy_pass_request_headers on;
                proxy_pass_request_body on;
        }

        location = /root_mirror {
                rewrite ^ /;
                proxy_pass http://172.17.0.1:8081;
                proxy_pass_request_body on;
        }
}
```

# Destroy all

```
$ ./destroy.sh
```

Content

```
#! /bin/bash
docker ps -aq
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker rmi $(docker images -q)
```
