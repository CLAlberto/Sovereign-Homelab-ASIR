# Certificados SSL automáticos en NPMplus con Porkbun y CNAME comodín (DNS Challenge)

> **¿Por qué usar DNS Challenge?**  
> Usar el método **DNS Challenge** para los certificados Let's Encrypt permite emitir y renovar certificados **aunque tus servicios estén en una IP local (LAN)** y no sean accesibles públicamente desde internet. ¡Ideal para homelabs y servidores domésticos!.

---

## 🔐 Añadir la API de Porkbun para certificados SSL automáticos en NPMplus

1. **En NPMplus**, al añadir un nuevo certificado, activa la opción **Use a DNS Challenge**.
2. Selecciona **Porkbun** como proveedor de DNS.
3. Rellena el campo "Credentials File Content" con tu API key y secret:
    ```
    dns_porkbun_key=tu-api-key
    dns_porkbun_secret=tu-api-secret
    ```
4. Guarda y aplica el certificado.

---

## 🌐 Configuración de CNAME comodín en Porkbun

Para que todos tus subdominios creados en NPMplus funcionen sin tener que registrarlos uno a uno en Porkbun, añade un registro CNAME comodín en la configuración DNS de Porkbun:

| TYPE   | HOST                   | ANSWER                | TTL  |
|--------|------------------------|-----------------------|------|
| CNAME  | *.lospajaros18.top     | lospajaros18.top      | 600  |

Esto redirige **cualquier subdominio** automáticamente al dominio principal. Así, cualquier proxy host que crees en NPMplus funcionará al instante.


---

## ✅ Ventajas de este método

- **No necesitas abrir puertos ni exponer tu servidor a Internet**.
- **Puedes apuntar los dominios directamente a una IP local**, útil para servicios internos, pruebas o laboratorios domésticos.
- **Automatiza todos los certificados**: NPMplus usará la API de Porkbun para emitir y renovar SSL sin intervención manual.
- **Solo necesitas crear el CNAME `*` una vez**: todos los subdominios que crees en NPMplus funcionarán de inmediato.

---

## 📝 Notas importantes

- **Este método es perfecto para homelabs** o cualquier entorno que use IPs privadas o no quiera exponer servicios directamente a Internet, pero si lo quieres exponer a internet, igualmente valdría.

---

**En resumen:**  
1. Configura la API de Porkbun en NPMplus para el DNS Challenge.  
2. Añade el CNAME `*` en Porkbun apuntando a tu dominio.  
3. Crea los subdominios en NPMplus y todo funcionará con SSL y HTTPS válido, ¡aunque tu servidor esté en la red local!
