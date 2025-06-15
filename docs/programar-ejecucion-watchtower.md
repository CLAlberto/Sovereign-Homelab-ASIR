# 🕒 Programar la Ejecución de Watchtower

Para que **Watchtower** se ejecute automáticamente cada 24 horas a las **03:00 (hora local)**, añade la siguiente línea al `crontab` de tu usuario:

```cron
0 3 * * * cd /home/proteos/ProyectoASIR2425/docker/watchtower && docker compose up --force-recreate watchtower
```

---

## 🔧 Pasos para configurar el cron (si aún no está instalado)

1. **Actualizar los paquetes del sistema:**
   ```bash
   sudo apt update
   ```

2. **Instalar el servicio `cron`:**
   ```bash
   sudo apt install cron
   ```

3. **Habilitar y arrancar `cron` de inmediato:**
   ```bash
   sudo systemctl enable --now cron
   ```

4. **Editar el `crontab` del usuario actual:**
   ```bash
   crontab -e
   ```

---

## 🌍 Asegurar la configuración horaria

Verifica que la zona horaria del sistema esté correctamente definida.  
Por ejemplo, para configurar la zona horaria de España:

```bash
sudo timedatectl set-timezone Europe/Madrid
```

Esto garantiza que la tarea programada se ejecute según la hora local deseada.

---

Con esto, tu servicio **Watchtower** se reiniciará diariamente a las 03:00 sin necesidad de intervención manual.
