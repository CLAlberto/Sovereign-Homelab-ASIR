# Configuración personalizada de Nginx en NPM Plus para Home Assistant

Para que Home Assistant funcione correctamente detrás de Nginx Proxy Manager (NPM Plus) con HTTPS y sin errores de cabecera o conexión, es necesario añadir unas líneas en la configuración avanzada del Proxy Host.

---

## 🛠️ ¿Dónde se pone?

1. Entra a **Nginx Proxy Manager Plus (NPM Plus)**.
2. Ve al menú **"Proxy Hosts"**.
3. Edita el Proxy Host correspondiente a tu dominio de Home Assistant (por ejemplo: `homeassistant.lospajaros18.top`).
4. Abre la pestaña **"Advanced"**.
5. En el campo **"Custom Nginx Configuration"**, pega lo siguiente:

```nginx
proxy_set_header Host $host;                             # Mantiene el dominio original en la cabecera
proxy_set_header X-Real-IP $remote_addr;                 # Pasa la IP real del cliente a Home Assistant
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  # Añade la cabecera estándar con IP original
proxy_set_header X-Forwarded-Proto $scheme;              # Indica si el acceso fue por HTTP o HTTPS
proxy_buffering off;                                     # Desactiva el buffer para evitar retardos en respuesta en tiempo real
