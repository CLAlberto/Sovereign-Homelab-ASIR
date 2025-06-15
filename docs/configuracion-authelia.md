# Documentación de configuración personalizada: Authelia

> **IMPORTANTE:**  
> Para que el sistema de autenticación y protección de servicios funcione correctamente, **es imprescindible personalizar y revisar** los archivos de configuración de Authelia antes de arrancar el contenedor en producción.

---

## 1. Archivos a revisar y editar

- **`configuration.yml.example`**
  - Contiene la configuración principal de Authelia (tema visual, redirecciones, claves secretas, reglas de acceso, sesiones, notificaciones, etc).
  - **Debes copiar este archivo como `configuration.yml` y adaptar:**
    - Las claves secretas:  
      - `jwt_secret`, `session.secret`, `storage.encryption_key`.  
      - **Genera claves fuertes y únicas** en [grc.com/passwords.htm](https://www.grc.com/passwords.htm) (elige las de 64 caracteres hexadecimales).
    - El dominio principal (ej: `lospajaros18.top`) y los subdominios protegidos.
    - Las reglas de acceso según tus servicios y el nivel de protección (one_factor, two_factor, bypass).
    - Los parámetros de sesión, notificaciones y almacenamiento local.

- **`users_database.yml.example`**
  - Define los usuarios autorizados, sus contraseñas hasheadas, grupos y correos electrónicos.
  - **Debes copiarlo como `users_database.yml` y crear/editar usuarios así:**
    - Cambia el nombre de usuario y `displayname` según convenga.
    - Genera la contraseña hasheada usando [argon2.online](https://argon2.online/) con los parámetros recomendados (parallelism 8, memory 1024, iterations 1, hash length 32).
    - Escribe el email y los grupos que correspondan.
    - Recuerda: **cada vez que cambies un usuario, debes reiniciar el contenedor de Authelia para aplicar los cambios.**

---

## 2. Reglas rápidas y buenas prácticas

- **No subas nunca los archivos `configuration.yml` ni `users_database.yml` reales al repositorio.**
  - Utiliza solo los `.example` para compartir plantillas seguras.
- Personaliza siempre los dominios, subdominios y políticas de acceso en función de los servicios desplegados y el nivel de seguridad deseado.
- Documenta qué política (two_factor, one_factor, bypass) aplica a cada subdominio/servicio en tu organización.
- Usa el modo `debug` en `log.level` solo durante la fase de pruebas.
- Cambia todas las claves secretas por valores aleatorios y únicos para cada despliegue.

---

## 3. Referencias útiles

- [Documentación oficial de Authelia](https://www.authelia.com/docs/configuration/)
- [Generador de contraseñas seguras (GRC)](https://www.grc.com/passwords.htm)
- [Generador de hash Argon2](https://argon2.online/)

---

## 4. Ejemplo de checklist para el despliegue

- [ ] Copiar los archivos `.example` a sus versiones definitivas y personalizarlos.
- [ ] Generar todas las claves y contraseñas desde cero.
- [ ] Adaptar los dominios, reglas y usuarios.
- [ ] Probar acceso, protección y 2FA en todos los servicios antes de pasar a producción.
- [ ] Documentar los cambios clave en este archivo para futuras referencias.

---

**Recuerda:**  
_“La seguridad no es un estado, es un proceso. Revisa y actualiza las configuraciones cada vez que cambien los servicios o el equipo.”_

---

