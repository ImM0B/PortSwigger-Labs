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
