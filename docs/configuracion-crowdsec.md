# Documentación de configuración personalizada: CrowdSec

> **IMPORTANTE:**  
> Antes de poner en marcha CrowdSec, **revisa y personaliza** el archivo `acquis.yaml` dentro de `docker/crowdsec/config/`, ya que aquí se define qué archivos de logs serán analizados para la detección de amenazas.

---

## 1. Preparación y estructura recomendada

La estructura de carpetas y los archivos `docker-compose.yml` de este repositorio están **preparados para funcionar sin modificaciones adicionales** tras clonar el proyecto.  
Las rutas de los logs que CrowdSec debe analizar se gestionan de forma **relativa**, lo que facilita la portabilidad, el despliegue automático y evita errores de configuración.

Esto significa que:
- Los logs de cada servicio (como NPMplus, Jellyfin, Transmission…) se guardan en carpetas estándar bajo la ruta `logs/` del proyecto.
- Tanto los servicios como CrowdSec acceden a estos logs mediante rutas relativas en los `docker-compose.yml` y en el propio `acquis.yaml`, por lo que no es necesario cambiar paths a mano tras clonar el repositorio.
- Si añades nuevos servicios y quieres protegerlos, sólo necesitas añadir su log a la carpeta correspondiente y actualizar `acquis.yaml`.

---

## 2. Archivo clave: `acquis.yaml`

Este archivo indica a CrowdSec qué logs debe monitorizar y bajo qué etiquetas los agrupa para su análisis y aplicación de escenarios de defensa.

### **¿Qué debes hacer?**
- **Asegúrate de que las rutas a los logs de cada servicio existen y son accesibles desde el contenedor.**
  - Por ejemplo, para analizar los logs de Jellyfin, Nginx o Transmission, debes mapear sus carpetas/archivos al contenedor de CrowdSec como se especifica en el `docker-compose.yml`.
- **Personaliza las rutas y etiquetas según tu infraestructura y los servicios que quieras proteger.**
- **Descomenta y añade logs nuevos** si incorporas servicios adicionales en tu entorno.

---

### **Estructura típica del archivo `acquis.yaml`:**

```yaml
filenames:
  - /logs/jellyfin/log*.log          # Todos los logs de Jellyfin (comodín *)
  - /logs/nginx/access.log           # Accesos de Nginx
  - /logs/nginx/error.log            # Errores de Nginx
  - /logs/nginx/stream.log           # Streaming de Nginx
  # - /logs/transmission/transmission.log   # Descomentar para analizar Transmission

labels:
  type: npmplus      # Etiqueta para asociar reglas y colecciones específicas
