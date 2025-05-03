
### ¿Qué es el `state` en OAuth?

Cuando una aplicación usa OAuth para autenticar a un usuario (por ejemplo, usando "Iniciar sesión con Google" o "Iniciar sesión con Facebook"), redirige al usuario al proveedor de identidad (como Google), y luego este lo redirige de vuelta a la aplicación con un "código de autorización".

El **parámetro `state`** es un valor que la aplicación original genera y envía junto con la solicitud de autenticación. Este valor:

- Es único y difícil de adivinar (por ejemplo, un hash relacionado con la sesión del usuario).
    
- Sirve como un **token CSRF** (protección contra ataques de falsificación de solicitudes).
    
- Se devuelve al final del flujo de OAuth, y la aplicación lo verifica para asegurarse de que la respuesta corresponde a la solicitud original.
    

---

### ¿Qué pasa si no se usa el parámetro `state`?

Si una aplicación **no incluye el parámetro `state`**, un atacante puede aprovechar esta omisión para realizar un ataque conocido como:

### 🔐 **Login CSRF** o **Forced OAuth Profile Linking**

Imagina este escenario:

1. El atacante inicia sesión en la aplicación con su propia cuenta de redes sociales (por ejemplo, su propia cuenta de Google).
    
2. La aplicación genera una URL de autenticación OAuth (sin `state`) y redirige al usuario a Google.
    
3. El atacante copia esta URL y se la envía a la víctima (por ejemplo, mediante phishing o incluyendo un enlace en una página web).
    
4. Si la víctima hace clic, su navegador completa el flujo de OAuth, pero el resultado es que **su cuenta local se vincula con la cuenta social del atacante**.
    

---

### ¿Por qué es peligroso?

Depende de cómo la aplicación maneje la autenticación:

- **Si permite vincular cuentas sociales:** La víctima podría perder el control de su cuenta, ya que ahora está vinculada al perfil social del atacante.
    
- **Si solo permite login vía OAuth:** La víctima podría iniciar sesión sin saberlo en la cuenta del atacante, exponiendo datos o realizando acciones sin querer.
    
___

**Authorization Code Grant Type** (response_type=code) :

Nos logueamos con cuenta social :
```HTTP
GET /auth?client_id=ajp0g9evvvjqpivusskiy&redirect_uri=https://0a0100780341076284a3bd4500940019.web-security-academy.net/oauth-login&response_type=code&scope=openid%20profile%20email
```

Capturamos la request que envía el authorization code y la dropeamos (ya que es de uso único) observamos que no hay status:

```HTTP
GET /oauth-login?code=V5NBkNtHNYp06f7WiA5YKajSlv_Xf29mwq70u1FDGwE
```

![[Pasted image 20250430114539.png]]

Se lo enviamos a la víctima mediante un iframe, hacemos log out , se lo volvemos a enviar  a la víctima y nos logueamos con la cuenta social.
Tenemos acceso a las funcionalidades de otra cuenta.

