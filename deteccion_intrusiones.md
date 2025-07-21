📁 deteccion_intrusiones.md

Objetivo del archivo:
Ofrecer un conjunto de herramientas y comandos orientados a detectar posibles accesos no autorizados, actividad sospechosa y señales de intrusión en un sistema Linux.

Te presento una introducción y después el contenido completo siguiendo la misma estructura que el archivo anterior (descripción, ejemplo, resultado y uso defensivo).

✅ Introducción
# 🔍 deteccion_intrusiones.md

Este documento agrupa comandos y herramientas útiles para detectar accesos no autorizados, cambios sospechosos o actividad anómala en sistemas Linux.

Identificar indicios de intrusión en sus primeras fases puede marcar la diferencia entre una respuesta eficaz y una brecha grave.

Aquí encontrarás comandos para:
- Analizar historial de accesos y actividad reciente.
- Inspeccionar procesos y conexiones inusuales.
- Detectar modificaciones en archivos críticos.
- Revisar logs relevantes para la seguridad.

> ⚠️ Consejo: muchos de estos comandos se deben ejecutar como superusuario para obtener información completa. Úsalos con precaución y en entornos controlados si estás aprendiendo.


🧰 Comandos para detección de intrusiones
## 🛠️ Comando: lastlog
📝 Muestra el último login de cada usuario del sistema.
💻 Ejemplo:
lastlog
📄 Resultado:
usuario1 pts/0 192.168.1.2 Mon Jul 21 09:00:00 +0000 2025
🔎 Uso defensivo: Detectar cuentas sin uso o logins desde ubicaciones inesperadas.

---

## 🛠️ Comando: last
📝 Muestra un historial de inicios de sesión recientes.
💻 Ejemplo:
last -n 10
📄 Resultado:
usuario1 pts/0 192.168.1.2 Mon Jul 21 09:00 still logged in
🔎 Uso defensivo: Identificar accesos remotos inusuales o en horarios atípicos.

---

## 🛠️ Comando: who / w
📝 Muestra usuarios actualmente conectados.
💻 Ejemplo:
who
📄 Resultado:
usuario1 pts/0 192.168.1.2 09:00
🔎 Uso defensivo: Confirmar conexiones activas, especialmente desde IPs sospechosas.

---

## 🛠️ Comando: grep "Accepted" /var/log/auth.log
📝 Filtra logins exitosos en el sistema.
💻 Ejemplo:
grep "Accepted" /var/log/auth.log | tail -n 5
📄 Resultado:
Jul 21 09:00:01 servidor sshd[1234]: Accepted password for usuario1 from 192.168.1.2
🔎 Uso defensivo: Ver accesos remotos legítimos o compromisos exitosos vía SSH.

---

## 🛠️ Comando: grep "Failed" /var/log/auth.log
📝 Filtra intentos fallidos de acceso.
💻 Ejemplo:
grep "Failed" /var/log/auth.log | tail -n 5
📄 Resultado:
Jul 21 09:00:01 servidor sshd[1234]: Failed password for invalid user root from 10.0.0.5
🔎 Uso defensivo: Detectar ataques de fuerza bruta o escaneos de credenciales.

---

## 🛠️ Comando: find / -perm -4000 -type f 2>/dev/null
📝 Busca archivos con permisos SUID.
💻 Ejemplo:
find / -perm -4000 -type f 2>/dev/null
📄 Resultado:
/usr/bin/passwd
/usr/bin/sudo
🔎 Uso defensivo: Revisar binarios privilegiados que pueden ser explotados para escalada de privilegios.

---

## 🛠️ Comando: netstat -anp / ss -anp
📝 Muestra conexiones activas con proceso asociado.
💻 Ejemplo:
ss -anp | grep ESTAB
📄 Resultado:
ESTAB 0 0 192.168.1.2:ssh 10.0.0.5:55412 users:(("sshd",pid=1234,fd=3))
🔎 Uso defensivo: Identificar conexiones sospechosas o procesos escuchando en puertos inusuales.

---

## 🛠️ Comando: lsof -nPi | grep LISTEN
📝 Lista servicios que están escuchando conexiones entrantes.
💻 Ejemplo:
lsof -nPi | grep LISTEN
📄 Resultado:
sshd 1234 root 3u IPv4 0x... TCP *:22 (LISTEN)
🔎 Uso defensivo: Ver qué servicios están accesibles desde red, especialmente si son inesperados.

---

## 🛠️ Comando: crontab -l / cat /etc/crontab
📝 Revisa tareas programadas por el usuario o el sistema.
💻 Ejemplo:
cat /etc/crontab
📄 Resultado:
@reboot root /bin/bash /tmp/script_sospechoso.sh
🔎 Uso defensivo: Detectar persistencia maliciosa mediante cron jobs.

---

## 🛠️ Comando: auditctl / ausearch
📝 Monitoriza eventos sensibles del sistema.
💻 Ejemplo:
ausearch -m USER_CMD -ts today
📄 Resultado:
registro de comandos ejecutados por usuarios con detalles de hora y terminal.
🔎 Uso defensivo: Auditar actividad sospechosa o uso indebido de privilegios.

---

## 🛠️ Comando: stat archivo
📝 Muestra fechas de creación, modificación y acceso.
💻 Ejemplo:
stat /etc/passwd
📄 Resultado:
Access: 2025-07-21
Modify: 2025-07-20
🔎 Uso defensivo: Detectar archivos críticos modificados fuera de horarios habituales.

---

## 🛠️ Comando: chkrootkit / rkhunter
📝 Herramientas para detectar rootkits conocidos.
💻 Ejemplo:
sudo chkrootkit
📄 Resultado:
Searching for Suckit rootkit... not found
🔎 Uso defensivo: Identificar herramientas de intrusión persistente.

---

## 🛠️ Comando: strings binario | grep /bin/sh
📝 Extrae cadenas de texto de binarios sospechosos.
💻 Ejemplo:
strings malware.bin | grep /bin/sh
📄 Resultado:
/bin/sh -c rm -rf /
🔎 Uso defensivo: Detectar payloads maliciosos dentro de binarios descargados.

---

## 🛠️ Comando: diff /etc/passwd /var/backups/passwd.bak
📝 Compara archivos críticos con copias de seguridad.
💻 Ejemplo:
diff /etc/passwd /var/backups/passwd.bak
📄 Resultado:
< usuariox:x:1002:1002::/home/usuariox:/bin/bash
🔎 Uso defensivo: Identificar cuentas nuevas creadas por intrusos.
