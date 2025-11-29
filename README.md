# Prometheus-and-Grafana

https://roadmap.sh/projects/monitoring

## How to use

update the IP adresses in [inventory](./inventory)

run ```ansible-playbook playbook.yml```

tou can access grafana at ```your_url/grafana``` and prometheus at ```your_url/prometheus```

default admin password is ```admin```


## Nginx conf

need this for the nginx exporter or cannot work (/etc/nginx/sites-available)

```
location /nginx_status {
    # Enable the stub status module to expose NGINX metrics
    stub_status on;
    # Only allow the local machine (the exporter) to access this endpoint
    allow 127.0.0.1; 
    deny all;
    # Turn off logging for this frequent scrape endpoint
    access_log off;
}

location /grafana/ {
    proxy_pass http://127.0.0.1:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}

location /prometheus/ {
    proxy_pass http://127.0.0.1:9090/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```
