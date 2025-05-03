https://portswigger.net/web-security/race-conditions#multi-endpoint-race-conditions

![[Pasted image 20250311214328.png]]


#### **Flujo normal de compra:**

1. Un usuario tiene productos en su carrito de compras (**basket pending**).
2. Envía una solicitud `POST /makePayment` para realizar el pago.
3. El sistema **valida el pago** y cambia el estado a **payment validated**.
4. Luego, la orden se **confirma** y los productos se envían (**basket confirmed**).

#### **Vulnerabilidad: Agregar más productos durante la validación**

Si el sistema no bloquea correctamente el carrito después del pago, un atacante puede explotar una condición de carrera de la siguiente manera:

1. El usuario inicia el pago (`POST /makePayment`).
2. Mientras el pago aún está en proceso (**race window** entre "payment validated" y "basket confirmed"), el atacante envía rápidamente otra solicitud para **agregar más productos** al carrito.
3. Si el sistema permite modificaciones antes de que el estado cambie a **basket confirmed**, los nuevos productos podrían ser incluidos sin un pago adicional.
4. Como resultado, el atacante paga solo por los productos originales, pero recibe todos los productos en el carrito.

![[{A0D804AE-DD08-404B-884E-4F8550AB60C1}.png]]

**Send group in pararell** sirve para enviar todas las request en el mismo paquete TCP, es la forma más efectiva de conseguir condiciones de carrera después del **Last Byte Sync**.

Notamos que si enviamos 2 request en secuencial en la misma conexión, siempre la primera request cuesta más que la segunda. Entonces ponemos una GET request a `/` como primera y luego una a `/checkout` y luego otra a `/cart` , ahora las enviamos en secuencial en la misma conexión y vemos que las últimas dos cuestan casi el mismo tiempo.

Esta demostración secuencial nos sirve para saber que cuando enviamos 2 request en pararelo (usando la técnica de Single Packet Attack, ya que es HTTP/2), casi llegarán a la vez (igual hay algún retraso del lado del servidor, pero eso no es importante)

```
- When sending over HTTP/1, Repeater uses last-byte synchronization. This is where multiple requests are sent over concurrent connections, but the last byte of each request in the group is withheld. After a short delay, these last bytes are sent down each connection simultaneously.

- When sending over HTTP/2+, Repeater sends the group using a single packet attack. This is where multiple requests are sent via a single TCP packet.
```

La solución está en enviar el POST a`/checkout` y el post a `/cart` a la vez mediante la técnica de single packet, habiendo ya algo en la cesta, como una tarjeta regalo.

