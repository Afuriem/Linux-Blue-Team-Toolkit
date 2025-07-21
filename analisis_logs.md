# 🧾 analisis_logs.md

Los registros del sistema (logs) son una fuente crítica de información en ciberseguridad defensiva. A través de ellos podemos reconstruir eventos, detectar accesos no autorizados, monitorizar servicios críticos y entender qué ocurrió antes, durante y después de un incidente.

Este documento no solo ofrece comandos, sino también **criterios para interpretar eventos sospechosos** en los logs más relevantes del sistema.

---

## 🗂️ Principales logs que debes conocer

| Archivo de log | Contenido | Interés defensivo |
|----------------|-----------|-------------------|
| `/var/log/auth.log` o `/var/log/secure` | Autenticaciones, sudo, SSH | Detección de accesos fallidos, escaladas, sesiones SSH |
| `/var/log/syslog` o `/var/log/messages` | Mensajes generales del sistema | Eventos de kernel, errores de servicios |
| `/var/log/faillog` | Fallos de login por usuario | Cuentas bloqueadas o atacadas |
| `/var/log/wtmp` y `btmp` | Logins exitosos y fallidos binarios | Se accede con `last` y `lastb` |
| `/var/log/audit/audit.log` | Auditoría avanzada (si auditd está activo) | Eventos críticos del sistema: comandos, cambios de permisos, etc. |

---

## 🔍 ¿Qué debes buscar en los logs?

Aquí tienes ejemplos de patrones **sospechosos o dignos de análisis**:

### 🔐 Autenticación
- Accesos desde IPs inusuales o países extranjeros.
- Muchos fallos de login seguidos de un login exitoso.
- Intentos con usuarios inválidos (`invalid user`).
- Uso inusual de `sudo`.

### 🧑‍💻 Comportamiento del sistema
- Reinicios de servicios clave sin justificación (`sshd`, `cron`, `rsyslog`...).
- Logs que desaparecen, rotan de forma anómala o han sido modificados.
- Alertas de `kernel` sobre procesos que se matan solos o cargan módulos sospechosos.

### 📦 Actividad de red
- Servicios escuchando en puertos que no deberían estar abiertos.
- Conexiones salientes desde procesos inesperados (por ejemplo, `bash` o `curl`).
- Tráfico constante hacia una misma IP (posible C2).

---

## 🧠 Buenas prácticas para analizar logs
No busques solo errores. Busca también comportamientos anómalos que no generen error (como un bash a medianoche).

Correlaciona eventos. Si ves un acceso SSH a las 02:00 y luego se reinicia el cron, puede ser parte de una intrusión.

Filtra por IP o usuario. Te ayudará a seguir el rastro de un atacante si repite patrón.

No ignores los logs binarios. Usa lastb y last para acceder a wtmp y btmp.

## 📌 Herramientas adicionales recomendadas
Logwatch: genera informes diarios de logs.

GoAccess: análisis en tiempo real de logs web (útil si hay Apache/Nginx).

Logcheck: analiza logs en busca de anomalías conocidas.

ELK / Graylog / Splunk: para sistemas con múltiples nodos o entornos productivos grandes.

> 🛡️ Recuerda: los logs cuentan una historia. Saber leerla es una de las habilidades más valiosas de cualquier analista de ciberseguridad defensiva.

---
## 🔧 Comandos útiles para buscar estos patrones

### 🛠️ Comando: journalctl
📝 Accede a todos los logs gestionados por systemd.
💻 Ejemplo:
journalctl -xe
📄 Resultado:
Jul 21 10:00:01 servidor sshd[1234]: Failed password for root from 10.0.0.5 port 55412
🔎 Uso defensivo: Consultar errores, accesos fallidos, reinicios de servicios o eventos críticos recientes.

---

### 🛠️ Comando: tail -f /var/log/syslog
📝 Visualiza logs en tiempo real.
💻 Ejemplo:
tail -f /var/log/auth.log
📄 Resultado:
Líneas que se van añadiendo en vivo según ocurren los eventos.
🔎 Uso defensivo: Monitorizar intentos de login, alertas del sistema o respuestas ante incidentes.

---

### 🛠️ Comando: grep "término" archivo.log
📝 Busca coincidencias en archivos de log.
💻 Ejemplo:
grep "Failed password" /var/log/auth.log
📄 Resultado:
Jul 21 10:01: sshd[2345]: Failed password for invalid user admin from 192.168.1.10
🔎 Uso defensivo: Detectar intentos de acceso fallido o brute force.

---

### 🛠️ Comando: grep -iE "error|fail|denied" archivo.log
📝 Busca múltiples patrones con sensibilidad a mayúsculas/minúsculas.
💻 Ejemplo:
grep -iE "error|fail|denied" /var/log/syslog
📄 Resultado:
Mensajes del sistema indicando fallos o denegaciones.
🔎 Uso defensivo: Encontrar pistas sobre errores de servicios o acciones bloqueadas por el sistema.

---

### 🛠️ Comando: zgrep "ssh" /var/log/auth.log.1.gz
📝 Busca en logs comprimidos.
💻 Ejemplo:
zgrep "Accepted" /var/log/auth.log.1.gz
📄 Resultado:
Líneas de log antiguas donde se aceptaron conexiones SSH.
🔎 Uso defensivo: Analizar eventos históricos que ya fueron archivados.

---

### 🛠️ Comando: awk '{print $1,$2,$3}' archivo.log
📝 Extrae campos específicos.
💻 Ejemplo:
awk '{print $1,$2,$3}' /var/log/auth.log | head
📄 Resultado:
Fecha y hora de los eventos logueados.
🔎 Uso defensivo: Generar líneas de tiempo o correlacionar eventos rápidamente.

---

### 🛠️ Comando: cut -d ' ' -f5- archivo.log
📝 Elimina columnas para enfocar el mensaje principal.
💻 Ejemplo:
cut -d ' ' -f5- /var/log/syslog | tail
📄 Resultado:
Muestra solo el contenido útil de cada entrada.
🔎 Uso defensivo: Limpiar ruido y centrarse en eventos concretos.

---

### 🛠️ Comando: sed -n '/Jul 21/,/Jul 22/p' archivo.log
📝 Muestra logs entre dos fechas.
💻 Ejemplo:
sed -n '/Jul 20/,/Jul 21/p' /var/log/auth.log
📄 Resultado:
Eventos que ocurrieron entre esas fechas.
🔎 Uso defensivo: Delimitar el análisis a la ventana temporal de un incidente.

---

### 🛠️ Comando: less +F archivo.log
📝 Visualiza log con scroll en tiempo real (modo seguimiento).
💻 Ejemplo:
less +F /var/log/syslog
📄 Resultado:
Salida similar a `tail -f`, pero navegable.
🔎 Uso defensivo: Revisar logs en vivo sin perder posibilidad de buscar o pausar.

---

### 🛠️ Comando: logwatch / logcheck (si están instalados)
📝 Herramientas para generar resúmenes automáticos de logs.
💻 Ejemplo:
sudo logwatch --detail High --service sshd --range today
📄 Resultado:
Informe con eventos importantes del día relacionados con SSH.
🔎 Uso defensivo: Automatizar el análisis diario de eventos relevantes de seguridad.

---

### 🛠️ Comando: aureport
📝 Resumen de eventos del sistema auditado.
💻 Ejemplo:
aureport --summary
📄 Resultado:
Cantidad de eventos por tipo (ej. autenticación, comandos ejecutados, etc.)
🔎 Uso defensivo: Obtener una visión rápida de las acciones realizadas en el sistema.

---

### 🛠️ Comando: diff archivo.log copia.log
📝 Compara dos versiones de un log.
💻 Ejemplo:
diff /var/log/auth.log /home/usuario/auth_bk.log
📄 Resultado:
Muestra diferencias línea por línea.
🔎 Uso defensivo: Detectar manipulación de logs o alteraciones posteriores a un incidente.

---

### 🛠️ Comando: find /var/log -type f -size +10M
📝 Identifica logs que han crecido demasiado.
💻 Ejemplo:
find /var/log -type f -size +10M
📄 Resultado:
/var/log/syslog.1
🔎 Uso defensivo: Detección de posibles ataques DoS o generación masiva de logs como técnica evasiva.
