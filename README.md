Dockerized OpenResty
===============

Optimised and secured SSL-only OpenResty build.
The configuration [follows best practices](https://github.com/h5bp/server-configs-nginx/) in performance and security plus additional changes.

##### Features

- Uses latest OpenSSL 1.01j
- Allows only the TLSv1 TLSv1.1 TLSv1.2 protocols ([see SSLv3 Poodle Vulnerability](https://www.us-cert.gov/ncas/alerts/TA14-290A))
- Server signature hidden (`Server` header removed)
- Additional headers (`X-Frame-Options`, `X-XSS-Protection`, `Strict-Transport-Security`)
- Open file cache
- `default_server` sends back HTTP 444 No Response
- Default cache settings for assets (fonts, images, js, css)
- Forbid access to files that could hang around (md, sql, sh, txt, tar, log, ini...)

The following modules are enabled:

- luajit
- pcre-jit
- srcache-nginx-module
- array-var-nginx-module
- encrypted-session
- echo-nginx-module
- redis2-nginx-module
- set-misc-nginx-module
- headers-more-nginx-module
- http_stub_status_module
- http_secure_link_module
- http_gzip_static_module
- http_gunzip_module
- http_ssl_module
- http_spdy_module
- pagespeed

### How to use

##### Configure your vhost
```nginx
server {
  listen [::]:443 ssl spdy;
  listen 443 ssl spdy;

  server_name example.com;

  ssl on;
  ssl_certificate /etc/nginx/certs/example.crt;
  ssl_certificate_key /etc/nginx/certs/example.key;

  # Include base configuration
  include base;

  # Path for static files
  root /var/www/example.com/public;
}
```

##### Run the container

```bash
# run the container & mount the volumes
docker run -d -p 443:443 \
    -v /etc/nginx/sites-enabled:/etc/nginx/sites-enabled \
    -v /etc/nginx/certs:/etc/nginx/certs \
    -v /etc/nginx/logs:/var/log/nginx \
    --name openresty \
    jgautheron/openresty
```