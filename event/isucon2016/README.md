# isucon 2016 memo

## nginx
``nginx
worker_processes  1;

events {
  worker_connections  1024;
}

user isucon;
http {
  upstream app {
    server 127.0.0.1:8080;
  }

  log_format with_time '$remote_addr - $remote_user [$time_local] '
                     '"$request" $status $body_bytes_sent '
                                          '"$http_referer" "$http_user_agent" $request_time';
  access_log /var/log/nginx/access.log with_time;
  upstream backend {
           server unix:/tmp/nginx.sock;
           }

server {
    location / {
      proxy_set_header Host $host;
      # proxy_pass http://app;                                                                                                                                
      proxy_pass http://backend;
    }
    location /nytprof {
             alias /tmp/nytprof;
    }
  }
}
``
