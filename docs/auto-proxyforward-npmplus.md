# Auto-proxyforward de NPMplus (Nginx Proxy Manager detrás de sí mismo)

> ⚠️ **IMPORTANTE:**  
> Exponer el propio panel de administración de NPMplus (o Nginx Proxy Manager) detrás de su propio proxy inverso suele generar **errores de redirección, bucles de login o fallos de SSL**.  
> Esta situación **no está bien documentada en la documentación oficial** y es habitual encontrar foros donde se afirma que “no tiene solución”.  
> Sin embargo, **con estos parámetros probados por mí** garantizo que funciona perfectamente.

---

## 🔧 ¿Para qué sirve este ajuste?

Cuando quieres acceder al panel de administración de NPMplus a través de un subdominio protegido por el propio proxy inverso (por ejemplo: `https://nginx.lospajaros18.top`), es imprescindible **ajustar la configuración avanzada** del proxy host para que gestione correctamente la validación SSL, el nombre del servidor y los reemplazos de URL.

---

## ⚙️ Custom configuration (Advanced) para NPMplus como proxy de sí mismo

En la configuración del **Proxy Host** para el dominio donde está el propio NPMplus, añade este bloque en la pestaña “Advanced” o “Custom Nginx Configuration”:

```nginx
proxy_ssl_verify off;
proxy_ssl_server_name on;
proxy_ssl_name $host;
sub_filter ':81/' '/';
sub_filter_once off;
proxy_redirect http://nginx.lospajaros18.top:81/ https://nginx.lospajaros18.top/;
