# 🔐 Clave secreta para DailyTxT

Para que el servicio **DailyTxT** funcione correctamente y mantenga la seguridad en el sistema de autenticación, es necesario generar una clave secreta (`SECRET_KEY`). Esta clave se utiliza para firmar los tokens JWT y debe ser **única y segura**.

## 🛠 Cómo generar la clave

Ejecuta el siguiente comando en tu terminal:

```bash
openssl rand -base64 64

Ejemplo: LqS6SRTtQ9CEZUn7A9ZbUQaXzH0IwU8ymT8dSHrRGrWr3q0AM30Mn9u8bZ+DyQ61eh+g5mgvqFwKHtVkGHMdxw==
