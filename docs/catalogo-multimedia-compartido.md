# Configuración de Catálogo multimedia Compartido para Contenedores Multimedia

> **IMPORTANTE:**  
> La ruta de la biblioteca multimedia está **compartida** entre los siguientes servicios:  
> - Jellyfin  
> - Lidarr  
> - Radarr  
> - Sonarr  
> - Transmission

---

## 📁 Variable de entorno utilizada

Todos estos contenedores utilizan una variable en su archivo `.env`:

```env
LIBRARY=../../library/
```

Esta ruta relativa indica que el directorio `library` se encuentra dos niveles por encima de donde está ubicado el archivo `.env`.

---

## 🎯 ¿Por qué se usa esta estructura?

- **Consistencia:** Todos los servicios acceden a la misma carpeta física.
- **Automatización:** Permite que Transmission descargue los archivos donde Radarr, Sonarr o Lidarr los esperan.
- **Compatibilidad:** Jellyfin puede escanear y reproducir directamente el contenido descargado.
- **Mantenimiento sencillo:** No hay rutas absolutas, lo cual facilita la portabilidad entre entornos.

---

## ⚠️ Advertencia

> **No modifiques esta ruta en los archivos `.env` ni en los volúmenes del `docker-compose.yml` a menos que se actualicen todos los servicios al mismo tiempo.**  
> Un cambio en un solo contenedor podría romper la sincronización y causar errores como:
>
> - Librería vacía en Jellyfin  
> - Fallos al mover archivos tras la descarga  
> - Desincronización entre descargadores y organizadores de contenido

