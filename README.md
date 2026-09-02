# 🛡️ Reporte de Laboratorio: Detección y Análisis de Fuerza Bruta en RDP

**Fecha:** 2 de Septiembre, 2026  
**Autor:** Tec. Michel Cueto  
**Perfil / Rol:** Analista de Ciberseguridad (Blue / Purple Team)  
**Entorno / Plataforma:** Home Lab Local  
**Dificultad / Tipo:** Principiante / Análisis de Logs / Hardening

---

## 1. Resumen Ejecutivo

Durante este ejercicio práctico de laboratorio se simuló un ataque de fuerza bruta dirigido al puerto RDP (3389) de un entorno Windows. El objetivo principal fue auditar los eventos del sistema, identificar el vector de ataque y aplicar medidas de *hardening* (endurecimiento de seguridad) para mitigar accesos no autorizados en la infraestructura local.

---

## 2. Topología del Laboratorio

* **Atacante (Kali Linux):** IP `192.168.1.50`
* **Víctima (Windows Server / 11):** IP `192.168.1.10`
* **Monitoreo:** Event Viewer / Registros de Auditoría de Windows

---

## 3. Metodología y Hallazgos Técnicos

### Paso 1: Detección del Intento de Intrusion
* **Evidencia de Auditoría:**
![Registro Event ID 4625](evidencia-de-eventos.png)
Se registró una ráfaga inusual de intentos de inicio de sesión fallidos en el sistema objetivo.

* **Event ID identificado:** `4625` (An account failed to log on).
* **Frecuencia:** Múltiples solicitudes por segundo desde la dirección `192.168.1.50`.
* ---
Comando utilizado en PowerShell para consultar eventos de inicio de sesión fallidos:

Get-EventLog -LogName Security -InstanceId 4625 | Select-Object TimeGenerated, Message -First 10
## 4. Plan de Remediación y Mitigación

Para neutralizar el vector de ataque se implementaron las siguientes directivas:

1. **Política de Bloqueo de Cuentas (Account Lockout Policy):** Se configuró la directiva para bloquear cuentas locales tras 5 intentos fallidos consecutivos durante un período de 15 minutos.
2. **Hardening de Servicios:** Deshabilitación de accesos RDP directos sin autenticación previa a nivel de red (NLA).

---

## 5. Conclusión

Este laboratorio permitió validar la importancia del monitoreo continuo de los Event Logs de Windows y la aplicación estricta de políticas de contraseñas para evitar compromisos mediante ataques automatizados.
