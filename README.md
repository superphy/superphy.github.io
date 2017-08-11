# superphy.github.io
Built using https://github.com/superphy/reactapp

# Instructions
1. Setup Certbot on your remote server running the Flask API / backend stack https://github.com/superphy/backend.
  ```
  https://certbot.eff.org/all-instructions/#ubuntu-16-04-xenial-nginx
  ```
2. Setup Nginx
```nginx
    server {
        client_max_body_size 60000m;
        listen       443 default_server ssl http2;
        listen       [::]:443 ssl http2;

        server_name spfy.enchartus.ca;

        ssl_certificate /etc/letsencrypt/live/spfy.enchartus.ca/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/spfy.enchartus.ca/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot

              location / {
                      proxy_pass http://localhost:8000;
                      proxy_set_header    Host             $host;
                      proxy_set_header    X-Real-IP        $remote_addr;
                      proxy_set_header    X-Forwarded-For  $proxy_add_x_forwarded_for;
                      proxy_set_header    X-Client-Verify  SUCCESS;
                      proxy_set_header    X-Client-DN      $ssl_client_s_dn;
                      proxy_set_header    X-SSL-Subject    $ssl_client_s_dn;
                      proxy_set_header    X-SSL-Issuer     $ssl_client_i_dn;
              }
      }
```
3. In Reactapp, edit `src/middleware/api.js` like so:
    ```
    const ROOT = 'https://spfy.enchartus.ca/'
    ```
4. Build:
    ``` sh
    yarn build
    ```
