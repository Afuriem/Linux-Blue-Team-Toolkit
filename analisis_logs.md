# 📁 analisis_logs.md

Objetivo del archivo:
Proporcionar comandos y técnicas para consultar, filtrar y entender los logs del sistema Linux, con enfoque en detección de eventos anómalos o sospechosos.

# ✅ Introducción
# 🧾 analisis_logs.md

El análisis de logs es una de las herramientas más valiosas del equipo Blue Team. Nos permite reconstruir eventos, identificar accesos sospechosos, evaluar comportamientos inusuales y detectar intentos de intrusión que no hayan sido bloqueados directamente.

Este documento recopila comandos y estrategias para revisar logs del sistema de forma efectiva, filtrando lo importante y detectando patrones relevantes para ciberseguridad defensiva.

Se incluyen:
- Logs comunes en sistemas Linux.
- Comandos para leer, buscar y extraer información útil.
- Casos de uso típicos en análisis post-incidente.

> 🛡️ Consejo: Si dominas los logs, dominas el sistema. Aprende a leer entre líneas para encontrar señales que otros pasarían por alto.

# 🧰 Comandos para análisis de logs
## 🛠️ Comando: journalctl
📝 Accede a todos los logs gestionados por systemd.
💻 Ejemplo:
journalctl -xe
📄 Resultado:
Jul 21 10:00:01 servidor sshd[1234]: Failed password for root from 10.0.0.5 port 55412
🔎 Uso defensivo: Consultar errores, accesos fallidos, reinicios de servicios o eventos críticos recientes.

---

## 🛠️ Comando: tail -f /var/log/syslog
📝 Visualiza logs en tiempo real.
💻 Ejemplo:
tail -f /var/log/auth.log
📄 Resultado:
Líneas que se van añadiendo en vivo según ocurren los eventos.
🔎 Uso defensivo: Monitorizar intentos de login, alertas del sistema o respuestas ante incidentes.

---

## 🛠️ Comando: grep "término" archivo.log
📝 Busca coincidencias en archivos de log.
💻 Ejemplo:
grep "Failed password" /var/log/auth.log
📄 Resultado:
Jul 21 10:01: sshd[2345]: Failed password for invalid user admin from 192.168.1.10
🔎 Uso defensivo: Detectar intentos de acceso fallido o brute force.

---

## 🛠️ Comando: grep -iE "error|fail|denied" archivo.log
📝 Busca múltiples patrones con sensibilidad a mayúsculas/minúsculas.
💻 Ejemplo:
grep -iE "error|fail|denied" /var/log/syslog
📄 Resultado:
Mensajes del sistema indicando fallos o denegaciones.
🔎 Uso defensivo: Encontrar pistas sobre errores de servicios o acciones bloqueadas por el sistema.

---

## 🛠️ Comando: zgrep "ssh" /var/log/auth.log.1.gz
📝 Busca en logs comprimidos.
💻 Ejemplo:
zgrep "Accepted" /var/log/auth.log.1.gz
📄 Resultado:
Líneas de log antiguas donde se aceptaron conexiones SSH.
🔎 Uso defensivo: Analizar eventos históricos que ya fueron archivados.

---

## 🛠️ Comando: awk '{print $1,$2,$3}' archivo.log
📝 Extrae campos específicos.
💻 Ejemplo:
awk '{print $1,$2,$3}' /var/log/auth.log | head
📄 Resultado:
Fecha y hora de los eventos logueados.
🔎 Uso defensivo: Generar líneas de tiempo o correlacionar eventos rápidamente.

---

## 🛠️ Comando: cut -d ' ' -f5- archivo.log
📝 Elimina columnas para enfocar el mensaje principal.
💻 Ejemplo:
cut -d ' ' -f5- /var/log/syslog | tail
📄 Resultado:
Muestra solo el contenido útil de cada entrada.
🔎 Uso defensivo: Limpiar ruido y centrarse en eventos concretos.

---

## 🛠️ Comando: sed -n '/Jul 21/,/Jul 22/p' archivo.log
📝 Muestra logs entre dos fechas.
💻 Ejemplo:
sed -n '/Jul 20/,/Jul 21/p' /var/log/auth.log
📄 Resultado:
Eventos que ocurrieron entre esas fechas.
🔎 Uso defensivo: Delimitar el análisis a la ventana temporal de un incidente.

---

## 🛠️ Comando: less +F archivo.log
📝 Visualiza log con scroll en tiempo real (modo seguimiento).
💻 Ejemplo:
less +F /var/log/syslog
📄 Resultado:
Salida similar a `tail -f`, pero navegable.
🔎 Uso defensivo: Revisar logs en vivo sin perder posibilidad de buscar o pausar.

---

## 🛠️ Comando: logwatch / logcheck (si están instalados)
📝 Herramientas para generar resúmenes automáticos de logs.
💻 Ejemplo:
sudo logwatch --detail High --service sshd --range today
📄 Resultado:
Informe con eventos importantes del día relacionados con SSH.
🔎 Uso defensivo: Automatizar el análisis diario de eventos relevantes de seguridad.

---

## 🛠️ Comando: aureport
📝 Resumen de eventos del sistema auditado.
💻 Ejemplo:
aureport --summary
📄 Resultado:
Cantidad de eventos por tipo (ej. autenticación, comandos ejecutados, etc.)
🔎 Uso defensivo: Obtener una visión rápida de las acciones realizadas en el sistema.

---

## 🛠️ Comando: diff archivo.log copia.log
📝 Compara dos versiones de un log.
💻 Ejemplo:
diff /var/log/auth.log /home/usuario/auth_bk.log
📄 Resultado:
Muestra diferencias línea por línea.
🔎 Uso defensivo: Detectar manipulación de logs o alteraciones posteriores a un incidente.

---

## 🛠️ Comando: find /var/log -type f -size +10M
📝 Identifica logs que han crecido demasiado.
💻 Ejemplo:
find /var/log -type f -size +10M
📄 Resultado:
/var/log/syslog.1
🔎 Uso defensivo: Detección de posibles ataques DoS o generación masiva de logs como técnica evasiva.
