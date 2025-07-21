# 📁 comandos_basicos.md

Este documento recopila comandos fundamentales de Linux útiles para cualquier analista defensivo. Son la base para interactuar con el sistema, revisar su estado general y entender lo que ocurre en tiempo real.

Aunque puedan parecer simples, estos comandos son esenciales para:

- Diagnosticar comportamientos anómalos rápidamente.
- Realizar una primera inspección forense.
- Automatizar tareas defensivas en scripts.
- Familiarizarse con los entornos Linux utilizados en entornos empresariales y SOCs.

Cada comando incluye:
- 📝 Descripción funcional.
- 💻 Ejemplo de uso.
- 📄 Resultado esperado o captura simulada.
- 🔎 Casos de uso defensivos.

> 💡 Consejo: no subestimes el poder de lo básico. Dominar estos comandos te hará mucho más ágil a la hora de responder ante incidentes o detectar anomalías en sistemas Linux.


## 🧠 Comandos Básicos – Navegación y Gestión de Archivos

### 🛠️ Comando: pwd
📝 Muestra el directorio actual.
💻 Ejemplo:
pwd
📄 Resultado:
/home/usuario
🔎 Uso defensivo: Verifica la ubicación actual antes de realizar acciones administrativas o de análisis.

---

### 🛠️ Comando: ls -l
📝 Lista archivos con permisos, propietario y tamaño.
💻 Ejemplo:
ls -l
📄 Resultado:
-rw-r--r-- 1 root root  4096 jul 21 10:00 archivo.txt
🔎 Uso defensivo: Detectar archivos con permisos inusuales o propietarios inesperados.

---

### 🛠️ Comando: cd
📝 Cambia de directorio.
💻 Ejemplo:
cd /etc
📄 Resultado:
(navega al directorio /etc)
🔎 Uso defensivo: Navegar por rutas sensibles como /etc o /var/log.

---

### 🛠️ Comando: cat, less, more
📝 Visualiza contenido de archivos.
💻 Ejemplo:
less /etc/passwd
📄 Resultado:
(despliega contenido del archivo con navegación)
🔎 Uso defensivo: Revisar contenido de archivos críticos sin modificarlos.

---

### 🛠️ Comando: cp, mv, rm
📝 Copiar, mover y eliminar archivos.
💻 Ejemplo:
cp archivo.txt /tmp/
📄 Resultado:
(copia archivo.txt al directorio /tmp/)
🔎 Uso defensivo: Respaldar archivos antes de analizarlos o modificarlos.

---

### 🛠️ Comando: mkdir, rmdir
📝 Crear y eliminar directorios.
💻 Ejemplo:
mkdir nueva_carpeta
📄 Resultado:
(se crea un nuevo directorio llamado 'nueva_carpeta')
🔎 Uso defensivo: Organizar archivos de evidencia o logs durante análisis.

---

### 🛠️ Comando: find /ruta -name archivo
📝 Busca archivos por nombre.
💻 Ejemplo:
find /etc -name "shadow"
📄 Resultado:
/etc/shadow
🔎 Uso defensivo: Localizar archivos críticos o ocultos que puedan estar comprometidos.

---

### 🛠️ Comando: file
📝 Muestra el tipo de un archivo.
💻 Ejemplo:
file archivo.bin
📄 Resultado:
archivo.bin: ELF 64-bit LSB executable
🔎 Uso defensivo: Identificar archivos sospechosos o renombrados (por ejemplo, malware disfrazado de PDF).

---

# 🔍 Monitoreo del sistema y análisis de procesos

### 🛠️ Comando: top, htop
📝 Monitorea procesos activos en tiempo real.
💻 Ejemplo:
top
📄 Resultado:
(lista dinámica de procesos con uso de CPU y RAM)
🔎 Uso defensivo: Detectar procesos inusuales que consumen muchos recursos.

---

### 🛠️ Comando: ps aux
📝 Lista todos los procesos en ejecución.
💻 Ejemplo:
ps aux
📄 Resultado:
usuario   1234  0.0  0.1 123456 1234 ? Ss  10:00  0:00 /usr/sbin/sshd
🔎 Uso defensivo: Detectar procesos maliciosos o usuarios no autorizados.

---

### 🛠️ Comando: netstat -tuln o ss -tuln
📝 Muestra puertos y conexiones activas.
💻 Ejemplo:
ss -tuln
📄 Resultado:
LISTEN 0 128 0.0.0.0:22 ...
🔎 Uso defensivo: Verificar servicios expuestos y detectar backdoors.

---

### 🛠️ Comando: lsof -i
📝 Muestra archivos abiertos y sockets de red.
💻 Ejemplo:
lsof -i
📄 Resultado:
sshd 1234 root 3u IPv4 0x... TCP *:22 (LISTEN)
🔎 Uso defensivo: Detectar procesos con conexiones sospechosas.

---

### 🛠️ Comando: who, w, last
📝 Muestra usuarios conectados y logins anteriores.
💻 Ejemplo:
last
📄 Resultado:
usuario pts/0 192.168.1.2 Mon Jul 21 09:00 still logged in
🔎 Uso defensivo: Revisar actividad de acceso y detectar sesiones no autorizadas.

---

### 🛠️ Comando: uptime, free -h, df -h
📝 Estado del sistema: tiempo encendido, RAM, disco.
💻 Ejemplo:
free -h
📄 Resultado:
              total        used        free
Mem:           7.8G        3.2G        4.6G
🔎 Uso defensivo: Evaluar carga del sistema y detectar consumos anómalos.

---

## 🔐 Auditoría, permisos y seguridad

### 🛠️ Comando: chmod, chown
📝 Cambia permisos y propiedad de archivos.
💻 Ejemplo:
chmod 600 archivo.txt
chown root:root archivo.txt
📄 Resultado:
(permisos seguros y propiedad cambiada a root)
🔎 Uso defensivo: Endurecer archivos sensibles como logs o credenciales.

---

### 🛠️ Comando: sudo
📝 Ejecuta comandos con privilegios elevados.
💻 Ejemplo:
sudo nano /etc/ssh/sshd_config
📄 Resultado:
(abre archivo con permisos de root)
🔎 Uso defensivo: Cambiar configuraciones críticas de forma segura.

---

### 🛠️ Comando: passwd
📝 Cambia la contraseña de un usuario.
💻 Ejemplo:
sudo passwd usuario
📄 Resultado:
(se solicita nueva contraseña)
🔎 Uso defensivo: Forzar cambios de clave tras compromisos.

---

### 🛠️ Comando: history
📝 Muestra historial de comandos ejecutados.
💻 Ejemplo:
history | grep netcat
📄 Resultado:
123  nc -lvp 4444
🔎 Uso defensivo: Detectar uso de herramientas maliciosas en sesiones anteriores.

---

### 🛠️ Comando: auditctl, ausearch
📝 Configura y consulta auditoría del sistema.
💻 Ejemplo:
ausearch -x /usr/bin/passwd
📄 Resultado:
(entry audit log sobre cambios de contraseña)
🔎 Uso defensivo: Revisar acciones sensibles como cambios de usuario o sudo.

---

### 🛠️ Comando: getfacl, setfacl
📝 Gestiona permisos extendidos.
💻 Ejemplo:
getfacl archivo.txt
📄 Resultado:
# file: archivo.txt
# owner: root
user::rw-
🔎 Uso defensivo: Verificar y aplicar controles de acceso detallados.

---

## 📦 Red y tráfico sospechoso

### 🛠️ Comando: tcpdump
📝 Captura paquetes de red.
💻 Ejemplo:
sudo tcpdump -i eth0 port 80
📄 Resultado:
(línea por cada paquete HTTP capturado)
🔎 Uso defensivo: Inspeccionar tráfico sospechoso en tiempo real.

---

### 🛠️ Comando: ifconfig o ip a
📝 Muestra interfaces de red y direcciones IP.
💻 Ejemplo:
ip a
📄 Resultado:
inet 192.168.1.10/24 brd 192.168.1.255 ...
🔎 Uso defensivo: Confirmar dirección IP activa y detectar interfaces falsas.

---

## 🛠️ Comando: ping, traceroute, nslookup, dig
📝 Diagnóstico de red.
💻 Ejemplo:
ping 8.8.8.8
📄 Resultado:
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.3 ms
🔎 Uso defensivo: Verificar conectividad y trazado de rutas sospechosas.

---

### 🛠️ Comando: curl, wget
📝 Descarga archivos o ejecuta peticiones HTTP.
💻 Ejemplo:
curl -I http://malicioso.com
📄 Resultado:
HTTP/1.1 200 OK
🔎 Uso defensivo: Inspeccionar cabeceras o probar URLs sin descargar el contenido.

---

## 🧾 Registros (Logs)

### 🛠️ Comando: journalctl
📝 Muestra logs gestionados por systemd.
💻 Ejemplo:
journalctl -xe
📄 Resultado:
(logs con detalles de errores o eventos críticos)
🔎 Uso defensivo: Investigar eventos recientes del sistema.

---

### 🛠️ Comando: tail -f /var/log/syslog
📝 Muestra logs en tiempo real.
💻 Ejemplo:
tail -f /var/log/auth.log
📄 Resultado:
línea a línea con autenticaciones o intentos fallidos.
🔎 Uso defensivo: Monitorear accesos sospechosos en vivo.

---

### 🛠️ Comando: grep, awk, cut, sed
📝 Filtrado y análisis de texto en logs.
💻 Ejemplo:
grep "Failed password" /var/log/auth.log
📄 Resultado:
registro de intentos fallidos de acceso SSH.
🔎 Uso defensivo: Detectar ataques de fuerza bruta o conexiones no deseadas.

---

### 🛠️ Comando: zcat, zgrep
📝 Lee archivos de logs comprimidos.
💻 Ejemplo:
zgrep "sshd" /var/log/auth.log.1.gz
📄 Resultado:
líneas con eventos relacionados a SSH en logs archivados.
🔎 Uso defensivo: Buscar incidentes pasados o actividad persistente.

---

## 🐚 Scripting y automatización

### 🛠️ Comando: bash script.sh
📝 Ejecuta un script de Bash.
💻 Ejemplo:
bash mantenimiento.sh
📄 Resultado:
(salida personalizada del script)
🔎 Uso defensivo: Automatizar tareas defensivas o de respuesta a incidentes.

---

### 🛠️ Comando: crontab -l, crontab -e
📝 Lista o edita tareas programadas.
💻 Ejemplo:
crontab -l
📄 Resultado:
0 2 * * * /scripts/backup.sh
🔎 Uso defensivo: Detectar persistencia o tareas maliciosas.

---

### 🛠️ Comando: echo, date, sleep
📝 Utilidades para scripting y pruebas.
💻 Ejemplo:
sleep 5 && echo "Listo"
📄 Resultado:
(muestra “Listo” tras 5 segundos)
🔎 Uso defensivo: Simular tareas y temporizadores en scripts automatizados.

---

## 🧪 Forense y análisis rápido

### 🛠️ Comando: stat archivo
📝 Muestra metadatos de un archivo.
💻 Ejemplo:
stat /etc/passwd
📄 Resultado:
Access, Modify, Change: fechas completas
🔎 Uso defensivo: Identificar cuándo fue accedido o modificado un archivo sensible.

---

### 🛠️ Comando: sha256sum, md5sum
📝 Calcula hash de archivos.
💻 Ejemplo:
sha256sum archivo.bin
📄 Resultado:
d2c2e3c...  archivo.bin
🔎 Uso defensivo: Verificar integridad o comparar con IOC conocidos.

---

### 🛠️ Comando: strings archivo
📝 Extrae texto legible de binarios.
💻 Ejemplo:
strings malware.bin | less
📄 Resultado:
(posibles rutas, comandos o direcciones dentro del binario)
🔎 Uso defensivo: Análisis básico de malware sin ejecución.

---

### 🛠️ Comando: file, hexdump
📝 Inspección avanzada de archivos.
💻 Ejemplo:
hexdump -C archivo.txt
📄 Resultado:
(salida hexadecimal + ASCII)
🔎 Uso defensivo: Revisar contenido oculto o sospechoso en archivos.
