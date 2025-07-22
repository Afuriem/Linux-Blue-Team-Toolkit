# 🌐 redes.md

El tráfico de red es una de las fuentes más valiosas de información para detectar actividad sospechosa, identificar servicios mal configurados y anticipar posibles compromisos.

Este documento recopila comandos esenciales para:
- Visualizar interfaces y conexiones activas.
- Detectar servicios que están escuchando en el sistema.
- Analizar tráfico entrante y saliente en tiempo real.
- Capturar paquetes para su análisis forense.
- Localizar comportamientos anómalos desde una perspectiva defensiva.

Los atacantes pueden ocultar procesos o modificar logs, pero rara vez pueden actuar sin generar tráfico. Un buen análisis de red puede ayudarte a detectar intrusiones silenciosas antes de que generen impacto.

> 🧠 Consejo: si solo vigilas lo que ocurre en disco, llegas tarde. En la red puedes ver lo que *está ocurriendo ahora*.

---

## 📌 Buenas prácticas para análisis de red

#### 1. Verifica puertos abiertos y escucha activa después de instalar software nuevo.
#### 2. Detecta procesos escuchando en puertos no estándar (ej: bash en el 8080).
#### 3. Supervisa conexiones salientes: ¿a qué IPs se conecta tu sistema?
#### 4. Si ves conexiones a IPs públicas desconocidas, investiga su reputación.
#### 5. Usa tcpdump para capturar sesiones sospechosas sin afectar el sistema.

### Recursos para investigar IPs externas:
####   - https://abuseipdb.com/
####   - https://www.virustotal.com/
####   - https://ipinfo.io/

---

> 🧠 Consejo: En ciberseguridad, la red nunca miente. Si entiendes a dónde va y de dónde viene el tráfico, puedes detectar intrusiones incluso antes de que se manifiesten por otros medios.

---

## 🔌 Información de interfaces y conectividad

#### Ver interfaces de red y direcciones IP
```bash
ip a
```
#### Ver tabla de rutas
```bash
ip route
```
#### Mostrar estadísticas y errores en interfaces
```bash
ip -s link
```
#### Comprobar conectividad (ping básico)
```bash
ping -c 4 8.8.8.8
```
#### Trazar la ruta de una conexión
```bash
traceroute 8.8.8.8
```
# Consultar resolución DNS
```bash
nslookup github.com
dig github.com
```
---

## 📡 Servicios escuchando y puertos abiertos

#### Ver servicios que están escuchando conexiones
```bash
ss -tuln
```
#### Mostrar procesos asociados a puertos
```bash
ss -tunlp
```
#### Alternativa clásica (menos recomendada)
```bash
netstat -tuln
```
#### Ver servicios con PID que están escuchando
```bash
lsof -nPi | grep LISTEN
```
#### Buscar servicios que escuchan en puertos fuera de lo normal
```bash
ss -tuln | grep -v ':22\|:80\|:443\|:53'
```

---

## 🔍 Conexiones activas y tráfico en tiempo real

#### Mostrar conexiones activas y procesos
```bash
ss -anptu
```
#### Ver conexiones establecidas con destino externo
```bash
ss -ant | grep ESTAB
```
#### Analizar sockets abiertos por procesos
```bash
lsof -i
```
#### Mostrar conexiones por puerto
```bash
ss -an | grep ':8080'
```

---

## 🧪 Captura y análisis de paquetes

#### Capturar paquetes de red en una interfaz (root)
```bash
sudo tcpdump -i eth0
```
#### Filtrar solo tráfico HTTP
```bash
sudo tcpdump -i eth0 port 80
```
#### Capturar tráfico hacia una IP concreta
```bash
sudo tcpdump dst 192.168.1.100
```
#### Ver tráfico que contiene una cadena específica
```bash
sudo tcpdump -A -i eth0 | grep "User-Agent"
```
#### Capturar y guardar a archivo para análisis posterior
```bash
sudo tcpdump -i eth0 -w captura.pcap
```

---

## 🎯 Detección de anomalías y tráfico sospechoso

#### Ver si hay conexiones persistentes a una IP externa
```bash
ss -ntp | grep ESTAB | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
```
#### Detectar conexiones salientes desde procesos sospechosos (ej: bash, curl, python)
```bash
lsof -i | grep -E "bash|curl|python"
```
#### Comprobar si el sistema está enviando datos al exterior
```bash
iftop -n (requiere instalación previa)
```
#### Ver estadísticas de tráfico por protocolo
```bash
netstat -s
```


