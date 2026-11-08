# Telegram ChatOps Infrastructure Controller

Sistema de gestión y control de infraestructura remota a través de Telegram, permitiendo consultar estados de servidores, ejecutar comandos autorizados y recibir alertas de red.

---

## Arquitectura y Componentes

- **Autenticación:** Lista blanca estricta de identificadores de usuario y comprobación de canal seguro.
- **Despacho:** Ejecución asíncrona de comandos de sistema con captura y sanitización de salidas.
- **Alertas:** Notificación interactiva ante caídas de servicios o anomalías de red.

---

## Instrucciones de Uso

Configure las variables locales en su entorno y ejecute `python telegram_kali_bot.py`.

---

## Consideraciones de Seguridad

- Este repositorio almacena código fuente, utilitarios de prueba y documentación técnica de ingeniería.
- Todas las credenciales, llaves privadas y variables de entorno deben suministrarse de forma local mediante archivos no versionados.
- Las herramientas de auditoría contenidas deben utilizarse exclusivamente en entornos autorizados y laboratorios de prueba.

---

**Autor:** Juan Carlos Araneda M.  
**Licencia:** MIT License (consulte [LICENSE](LICENSE))
