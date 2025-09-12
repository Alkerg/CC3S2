## 1) HTTP: Fundamentos y herramientas

### 1. Levanta la app con variables de entorno
![](./evidencias/logging1.png)

### 2. Inspección con curl
![](./evidencias/curl1.png)
![](./evidencias/curl2.png)
**¿Qué ocurre si no hay ruta/método?**
Si no se especifica la URL o el método aparece el mismo error indicando que no se ha especificado la **URL**

### 3. Puertos abiertos con ss
![](./evidencias/ss.png)

### 4. Logs como flujo

```
127.0.0.1 - - [12/Sep/2025 16:14:34] "GET / HTTP/1.1" 200 -
{"ts": "2025-09-12T16:15:48-0500", "level": "INFO", "event": "request", "method": "POST", "path": "/", "remote": "127.0.0.1", "pro127.0.0.1", "proto": "http"}
127.0.0.1 - - [12/Sep/2025 16:15:48] "POST / HTTP/1.1" 405 -
{"ts": "2025-09-12T16:24:22-0500", "level": "INFO", "event": "request", "method": "GET", "path": "/", "remote": "127.0.0.1", "prot27.0.0.1", "proto": "http"}
```

## 2) DNS: nombres, registros y caché

### 1. Host local
Usando el target de la guía **hosts-setup** para agregar ```127.0.0.1 miapp.local```
![](./evidencias/hosts1.png)

### 2. Comprueba resolución
```dig +short miapp.local```
![](./evidencias/dig1.png)
```getent hosts miapp.local```
![](./evidencias/getent1.png)

### 3. TTL/Caché (conceptual)
```dig example.com A +ttlunits```
![](./evidencias/ttl.png)


## 3) TLS: seguridad en tránsito con Nginx como reverse proxy

### 1. Certificado de laboratorio:
Usando el target ```tls-cert```
![](./evidencias/tls-cert.png)

## Preguntas guía
**HTTP: explica idempotencia de métodos y su impacto en retries/health checks. Da un ejemplo con curl -X PUT vs POST.**

**DNS: ¿por qué hosts es útil para laboratorio pero no para producción? ¿Cómo influye el TTL en latencia y uso de caché?**

**TLS: ¿qué rol cumple SNI en el handshake y cómo lo demostraste con openssl s_client?**

**12-Factor: ¿por qué logs a stdout y config por entorno simplifican contenedores y CI/CD?**

**Operación: ¿qué muestra ss -ltnp que no ves con curl? ¿Cómo triangulas problemas con journalctl/logs de Nginx?**
