# 🔍 deteccion_intrusiones.md

Detectar una intrusión a tiempo puede marcar la diferencia entre una anécdota y una brecha grave. Este documento reúne técnicas y comandos para identificar posibles accesos no autorizados o actividad anómala en sistemas Linux.

Más allá de los fallos de login o conexiones SSH, aquí aprenderás a detectar señales menos evidentes de compromiso, como tareas ocultas, persistencia mediante cron, escaladas de privilegios o binarios sospechosos.

---

## 🧩 Indicadores comunes de intrusión en Linux

| Indicador | ¿Por qué es sospechoso? |
|----------|--------------------------|
| ❌ Fallos de autenticación reiterados | Intentos de fuerza bruta o reconocimiento previo |
| 🕵️ Accesos remotos fuera de horario | Actividad que evita supervisión humana |
| 🧑‍💻 Nuevos usuarios sin justificación | Creación de cuentas persistentes |
| 🐚 Bash ejecutándose sin motivo aparente | Posible reverse shell |
| 🔄 Cron jobs inesperados | Mecanismos de persistencia |
| 🎯 Servicios escuchando en puertos inusuales | Backdoors, túneles, malware en escucha |
| 🧬 Binarios modificados o con permisos SUID | Escalada de privilegios |

---

## 🛡️ Comandos y técnicas de detección

### 🧑‍💼 Revisión de inicios de sesión

#### ✅ Ver últimos accesos por usuario
```bash
lastlog

# Ver últimos accesos por usuario
lastlog

# Accesos recientes con fecha y duración
last -F | head -n 10

# Sesiones activas actualmente
w

---

# 🚪 Autenticaciones SSH

# Logins exitosos vía SSH
grep "Accepted" /var/log/auth.log

# Fallos de autenticación más repetidos
grep "Failed password" /var/log/auth.log | sort | uniq -c | sort -nr | head

# Intentos con usuarios inválidos
grep "Invalid user" /var/log/auth.log

---

# 🔧 Persistencia y manipulación del sistema

# Cron jobs del sistema
cat /etc/crontab

# Cron jobs por usuario
for user in $(cut -f1 -d: /etc/passwd); do crontab -u $user -l 2>/dev/null; done

# Binarios con permisos SUID (escalada)
find / -perm -4000 -type f 2>/dev/null

---

# 🕸️ Actividad de red sospechosa

# Conexiones activas y puertos abiertos
ss -antup

# Servicios escuchando
lsof -nPi | grep LISTEN

---

# 🧠 Comprobaciones forenses rápidas

# Fechas de modificación de archivos críticos
stat /etc/passwd /etc/shadow /etc/ssh/sshd_config

# Comparación con backups
diff /etc/passwd /var/backups/passwd.bak

# Comandos ejecutados hoy (requiere auditd)
ausearch -m USER_CMD -ts today

---

# 🧪 Herramientas específicas de detección

# Rootkit Hunter
sudo rkhunter --check

# Chkrootkit
sudo chkrootkit

---

## 📌 Buenas prácticas

# Correlaciona logs: auth.log, syslog, cron, etc.
# Verifica conexiones con IP externas sospechosas:
#   -> https://abuseipdb.com/
#   -> https://www.virustotal.com/
# Si sospechas de compromiso:
#   - Apaga la máquina.
#   - Clona el disco.
#   - Analiza en entorno forense aislado.

# 🧠 Consejo:
# Una intrusión rara vez deja un solo rastro.
# Aprende a hilar pequeñas anomalías hasta formar un patrón claro.
