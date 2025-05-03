 A veces hay balanceadores de carga o proxys inversos mal configurados que redirigen tanto peticiones de webs expuestas a internet como de redes internas, ya que tienen acceso a ambas.


Esto se detecta poniendo en la cabecera nuestro burp collaborator :

```HTTP
Host: a7ij2a8j3vn6k1vsvcyde33lyc49s0vok.oastify.com
```

SI recibimos un DNS entonces podríamos mapear las webs privadas a las que tenemos acceso (SSRF)

**En 10.0.0.0/8**
**y 192.168.0.0/16**

```bash
prips 192.168.0.0 192.168.255.255 > ips.txt
```

![[Pasted image 20250503163351.png]]

Y hay que quitar también lo de url encodear

___
___



> **¿Por qué, cuando en Burp cambias la cabecera `Host`, la petición llega al _proxy inverso_ del sitio (como el de Web Security Academy) en lugar de ir directamente al dominio objetivo, aunque el dominio esté en el `Host`?**

---

## 🔍 La clave: cómo funciona HTTP vs. TCP/IP

La confusión común aquí está entre:

- El campo `Host:` del protocolo **HTTP**
    
- La dirección IP o dominio al que realmente se establece la conexión **TCP** (nivel de red)
    

---

### 🧠 Burp **NO** hace DNS ni conexiones basadas en el header `Host`

Cuando tú haces esta petición desde Burp:

```http
GET / HTTP/1.1
Host: ejemplo.com
```

Pero se la estás enviando a:

```plaintext
0a0d004d03444b2880c862f100f00035.web-security-academy.net
```

Entonces lo que pasa es:

### 1. 🔗 **Burp se conecta por TCP/IP a** `0a0d004d03444b2880c862f100f00035.web-security-academy.net`

- Este es el servidor real que se resuelve por DNS y con el que se establece la conexión de red (nivel bajo).
    
- Es donde vive el servidor o proxy inverso (NGINX, Apache, etc.)
    

### 2. 📩 **Dentro de esa conexión, Burp pone el header `Host: ejemplo.com`**

- Este encabezado es solo texto dentro del protocolo HTTP.
    
- El servidor receptor (proxy inverso) **ve ese `Host` y actúa en consecuencia**, por ejemplo:
    
    - Buscar el virtualhost correspondiente
        
    - Redirigir
        
    - Validar el nombre
        
    - Hacer una subpetición (como en SSRF o SSRF-like)
        
    - O incluso reenviar esa petición a otro servidor
        

---

## 🤯 Entonces, ¿por qué la petición llega a tu OAST?

Porque el **proxy inverso al que Burp hace la conexión** (por IP/DNS real) **procesa el `Host:` falso que tú le metiste** y **hace una nueva petición desde él hacia ese dominio `oastify.com`**.

Burp **nunca conecta directamente a `oastify.com`** en ese paso.

---

## 🧪 Ejemplo más claro

Supón:

```http
GET / HTTP/1.1
Host: evil.oastify.com
```

Pero la IP a la que realmente Burp conecta es:

```plaintext
0a0d004d03444b2880c862f100f00035.web-security-academy.net (resuelta por DNS)
```

Entonces:

- **Burp conecta a esa IP**
    
- Envia HTTP con `Host: evil.oastify.com`
    
- El servidor (proxy inverso o backend) **procesa ese `Host` como válido**
    
- Y puede hacer algo como:
    
    - Redirigir
        
    - Validar el dominio (incluso hacer un `GET` o `HEAD` interno)
        
    - Contactar a `evil.oastify.com`
        
- Y ahí es donde tú ves la petición en OAST: no viene de Burp, viene del backend que cayó en la trampa.
    

---
