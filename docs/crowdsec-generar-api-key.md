# Configuración de `crowdsec.conf` para NGINX Proxy Manager (NPM)

## 🔑 Cómo generar la API KEY del bouncer

1. Asegúrate de que el contenedor de CrowdSec está en funcionamiento.
2. Ejecuta el siguiente comando en tu terminal:

```bash
docker exec -it crowdsec cscli bouncers add npmplus-bouncer
```
3. Copia la clave generada y pégala en la línea `API_KEY=` del archivo `crowdsec.conf` en la carpeta de nginx-proxy-manager.


