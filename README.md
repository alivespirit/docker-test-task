# docker-test-task
Repository for creating 3 Docker containers:
 - 1 web with Nginx serving static content 
 - 1 log_agent with Rsyslog receiving Nginx logs from web and forwarding logs further to log_server
 - 1 log_server with Rsyslog storing logs to host machine

Requirements:
 - docker
 - docker-compose

Usage:
```
cd rsyslog/
docker-compose up
```

This will create 3 containers:
 - rsyslog_web_1
 - rsyslog_log_agent_1
 - rsyslog_log_server_1  

Static web content (`./nginx/html/`) is available on port 80, logs from web are forwarded by [nginx.conf configuration](rsyslog/nginx/conf/nginx.conf#L22-L24) to log_agent on port 515, and then forwarded to log_server on port 514. Nginx logs are available on host machine under `./log_server/nginx/nginx.log`.  

Logs are sent with `local0` facility and only `local0` logs are forwarded by log_agent to log_server.
  
Log_server address is in [log_agent/rsyslog.conf](rsyslog/log_agent/rsyslog.conf#L90), in case you need to change it - edit the file and run `docker-compose restart log_agent`.

If you need another web container, copy the `web` service configuration in [docker-compose.yml](rsyslog/docker-compose.yml) and update host port. Also it is possible to change `ports` directive to `expose` and add some proxy to be able to use `scale` functionality of docker-compose.


Tested on CentOS 7.4, Docker 17.09.0-ce, docker-compose 1.17.1.
