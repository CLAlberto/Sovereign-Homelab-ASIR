# NGINX Proxy Manager - Configuración personalizada para Authelia y los hosts


## **Pegar en el host proxy de authelia (sin comillas). No es necesario modificar si no se modifica el docker-compose.yaml de authelia**
" location / {
set $upstream_authelia http://authelia:9091; # Tenemos que poner el nombre del container de authelia:puerto elegido
proxy_pass $upstream_authelia;
client_body_buffer_size 128k;

#Timeout if the real server is dead
proxy_next_upstream error timeout invalid_header http_500 http_502 http_503;

# Advanced Proxy Config
send_timeout 5m;
proxy_read_timeout 360;
proxy_send_timeout 360;
proxy_connect_timeout 360;

# Basic Proxy Config
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-Host $http_host;
proxy_set_header X-Forwarded-Uri $request_uri;
proxy_set_header X-Forwarded-Ssl on;
proxy_redirect  http://  $scheme://;
proxy_http_version 1.1;
proxy_set_header Connection "";
proxy_cache_bypass $cookie_session;
proxy_no_cache $cookie_session;
proxy_buffers 64 256k;
proxy_buffer_size 128k;
proxy_busy_buffers_size 512k;

# If behind reverse proxy, forwards the correct IP, assumes you're using Cloudflare. Adjust IP for your Docker network.
set_real_ip_from 172.18.0.0/16; #Aqui tienes que poner el rango de tu red docker
real_ip_header CF-Connecting-IP;
real_ip_recursive on;
}
"









 ## **Pegar en el host y cambiar "$upstream_transmission http://transmission:9091;" por "$upstream_XXXXXXX http://XXXXXXX:YYYY;" Donde XXXXXXX es el nombre del contenedor del host y YYYY el puerto INTERNO del contenedor, esta config es un ejemplo, pero tal como está funciona perfectamente para transmission si no se cambia nada del docker-compose.yaml**
# Ruta interna que se usa para verificar la autenticación con Authelia
location /authelia {
    internal;

    # Dirección interna del contenedor de Authelia (puede ser IP o nombre del servicio Docker)
    set $upstream_authelia http://authelia:9091/api/verify; # ⚠️ Cambiar si Authelia está en otro puerto o IP

    proxy_pass_request_body off;
    proxy_pass $upstream_authelia;
    proxy_set_header Content-Length "";

    # Reintento si el servidor da error o no responde
    proxy_next_upstream error timeout invalid_header http_500 http_502 http_503;
    client_body_buffer_size 128k;

    # Cabeceras necesarias para que Authelia entienda la petición
    proxy_set_header Host $host;
    proxy_set_header X-Original-URL $scheme://$http_host$request_uri;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Host $http_host;
    proxy_set_header X-Forwarded-Uri $request_uri;
    proxy_set_header X-Forwarded-Ssl on;

    proxy_redirect http:// $scheme://;
    proxy_http_version 1.1;
    proxy_set_header Connection "";
    proxy_cache_bypass $cookie_session;
    proxy_no_cache $cookie_session;

    proxy_buffers 4 256k;
    proxy_buffer_size 128k;
    proxy_busy_buffers_size 512k;

    send_timeout 5m;
    proxy_read_timeout 240;
    proxy_send_timeout 240;
    proxy_connect_timeout 240;
}

# Ruta principal que quieres proteger con Authelia (por ejemplo, Transmission)
location / {

    # Dirección del servicio que quieres proteger (nombre del contenedor + puerto)
    set $upstream_transmission http://transmission:9091;  # ⚠️ Cambiar por el contenedor real (ej. http://jellyfin:8096)

    proxy_pass $upstream_transmission;

    # Llamada interna a /authelia para comprobar si el usuario tiene sesión válida
    auth_request /authelia;

    # Preparación de cabeceras personalizadas
    auth_request_set $target_url $scheme://$http_host$request_uri;
    auth_request_set $user $upstream_http_remote_user;
    auth_request_set $groups $upstream_http_remote_groups;

    # Cabeceras que se pasan al servicio protegido con los datos del usuario autenticado
    proxy_set_header Remote-User $user;
    proxy_set_header Remote-Groups $groups;

    # Si Authelia devuelve 401, redirige al login
    error_page 401 =302 https://auth.lospajaros18.top/?rd=$target_url; # ⚠️ Cambiar por tu subdominio real de Authelia si es distinto

    client_body_buffer_size 128k;

    proxy_next_upstream error timeout invalid_header http_500 http_502 http_503;

    send_timeout 5m;
    proxy_read_timeout 360;
    proxy_send_timeout 360;
    proxy_connect_timeout 360;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Host $http_host;
    proxy_set_header X-Forwarded-Uri $request_uri;
    proxy_set_header X-Forwarded-Ssl on;

    proxy_redirect http:// $scheme://;
    proxy_http_version 1.1;
    proxy_set_header Connection "";
    proxy_cache_bypass $cookie_session;
    proxy_no_cache $cookie_session;

    proxy_buffers 4 256k;
    proxy_buffer_size 128k;
    proxy_busy_buffers_size 512k;

    # ⚠️ Rango IP de tu red local o de la red Docker para obtener IP real del cliente
    set_real_ip_from 172.18.0.0/16; # Ejemplo: 192.168.1.0/24 o el rango de tu red de casa

    real_ip_header X-Forwarded-For;
    real_ip_recursive on;
}
