https://dev.to/sharathmohan007/nginx-configuration-for-angular2-deployment-2b94
https://angular.io/guide/deployment

[Nginx](https://nginx.org/)

Use `try_files`, as described in [Front Controller Pattern Web Apps](https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/#front-controller-pattern-web-apps), modified to serve `index.html`:

try_files $uri $uri/ /index.html;

____________________________________________ 
```
server{
    listen 80;
    listen [::] 80;
    server_name www.example.com example.com;
    root /var/www/example.com;
    index index.html;
    location / {
        try_files $uri$args $uri$args/ /index.html;
    }
}
```

