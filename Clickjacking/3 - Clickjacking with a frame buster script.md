Los frame buster son scripts que permiten detectar si estamos tratando de cargar un iframe malicioso.

---

## 🛡️ ¿Qué es un _frame buster_?

Un **frame buster** es un pequeño script de JavaScript que los sitios legítimos usan para **evitar que su contenido sea embebido dentro de un `<iframe>`**, como defensa contra ataques como el **clickjacking**.

Un ejemplo clásico de frame buster:

```javascript
if (window.top !== window.self) {
  window.top.location = window.location; // Intenta romper el frame y recargar en el top
}
```

Este script detecta si el sitio se está mostrando en un `<iframe>` y, si es así, **intenta "salir" del iframe** y cargar el sitio en toda la pestaña (top-level window).

---

## 🧨 ¿Qué problema tienen los frame busters?

- **Dependen de JavaScript**, así que si el navegador bloquea JS (por ejemplo, por configuraciones de privacidad), no funcionan.
    
- **No son universales**: no todos los navegadores o versiones se comportan igual.
    
- Los atacantes **pueden desactivarlos usando una técnica con HTML5 y el atributo `sandbox` en los iframes.**
    

---

## ⚠️ El truco del atacante: usar `<iframe sandbox>`

HTML5 introdujo el atributo `sandbox` para iframes, que **limita lo que puede hacer el contenido embebido**. Y aquí es donde está el truco.

Un iframe con sandbox como este:

```html
<iframe 
  src="https://victim-website.com" 
  sandbox="allow-forms">
</iframe>
```

Hace lo siguiente:

- **Desactiva muchas capacidades peligrosas** (como scripts, popups, navegación).
    
- **Permite solo lo que se indique explícitamente** con valores como:
    
    - `allow-scripts`: permite JavaScript.
        
    - `allow-forms`: permite envío de formularios.
        
    - `allow-same-origin`: permite que el iframe actúe como si fuera del mismo origen (si no se pone, se comporta como de origen cruzado).
        
    - `allow-top-navigation`: permite que el contenido dentro del iframe redirija la pestaña completa.
        

---

### 🔥 ¿Qué pasa si **NO se pone `allow-top-navigation`**?

Eso es lo importante.

➡️ Si no se incluye `allow-top-navigation`, **el código del frame buster no puede redirigir al `window.top`**.  
O sea, el script de protección del sitio víctima queda **neutralizado**, porque el iframe está bloqueado para navegar fuera de sí mismo, por normas del `sandbox`.

Entonces, aunque el frame buster se ejecute, **el navegador simplemente ignora el intento de redirección**, como medida de seguridad.

---

## 💀 Resultado: el atacante gana

Con esta configuración:

```html
<iframe src="https://victim.com" sandbox="allow-forms allow-scripts"></iframe>
```

- ✅ El iframe puede usar JavaScript.
    
- ✅ Puede mostrar formularios.
    
- ❌ Pero **NO puede salirse del iframe**, por lo tanto el frame buster no puede proteger la página.
    

---
___





Como resolver el lab : 

Cargamos el script en esta url y bypasseamos el frame buster usando sandbox iframe :

Como el buster de la página es este :

```javascript
<script>
    if(top != self) {
	    window.addEventListener("DOMContentLoaded", function() {
	        document.body.innerHTML = 'This page cannot be framed';
	    }, false);
    }
</script>
```

Entonces no habrá que permitir la ejecución de scripts para que el buster no moleste :

![[Pasted image 20250421154414.png]]

Pero si el buster fuese este : 

```javascript
if (window.top !== window.self) {
  window.top.location = window.location;
}
```

Entonces se podría permitir la ejecución de scripts porque aunque se cargase el script, no podría superponer la página original al decoy porque no estaría excepcionado `allow-top-navigation`

➡️ Si no se incluye `allow-top-navigation`, **el código del frame buster no puede redirigir al `window.top`**.  
O sea, el script de protección del sitio víctima queda **neutralizado**, porque el iframe está bloqueado para navegar fuera de sí mismo, por normas del `sandbox`.

Entonces, aunque el frame buster se ejecute, **el navegador simplemente ignora el intento de redirección**, como medida de seguridad.