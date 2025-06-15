# Permitir acceso a Home Assistant a través de Nginx Proxy Manager

Para que Home Assistant acepte correctamente las conexiones de Nginx Proxy Manager (NPM), debes añadir la IP del proxy como confiable en tu archivo `configuration.yaml`.

### 👇 Añade esto en tu `configuration.yaml`:

```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 192.168.3.10  # Sustituir por la IP real del servidor donde está instalado NPM (NO la IP del contenedor Docker)
    - 127.0.0.1     # Opcional, útil si accedes desde localhost
