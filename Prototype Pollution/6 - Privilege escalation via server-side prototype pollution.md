
```javascript
function toLabel(key) {
    const string = key.replaceAll("_", " ").trim();
    return string.charAt(0).toUpperCase() + string.slice(1);
}

function handleSubmit(event) {
    event.preventDefault();

    const data = new FormData(event.target);

    const value = Object.fromEntries(data.entries());

    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (this.readyState == 4) {
            const responseJson = JSON.parse(this.responseText);
            const form = document.querySelector('form[name="change-address-form"');
            const formParent = form.parentElement;
            form.remove();
            const div = document.createElement("div");
            if (this.status == 200) {
                const header = document.createElement("h3");
                header.textContent = 'Updated Billing and Delivery Address';
                div.appendChild(header);
                formParent.appendChild(div);
                for (const [key, value] of Object.entries(responseJson).filter(e => e[0] !== 'isAdmin')) {
                    const label = document.createElement("label");
                    label.textContent = `${toLabel(key)}`;
                    div.appendChild(label);
                    const p = document.createElement("p");
                    p.textContent = `${JSON.stringify(value).replaceAll("\"", "")}`;
                    div.appendChild(p);
                }
            } else {
                const header = document.createElement("h3");
                header.textContent = 'Error';
                div.appendChild(header);
                formParent.appendChild(div);
                const p = document.createElement("p");
                p.textContent = `${JSON.stringify(responseJson.error && responseJson.error.message) || 'Unexpected error occurred.'} Please login again and retry.`;
                div.appendChild(p);
            }
        }
    };
    var form = event.currentTarget;
    xhr.open(form.method, form.action);
    xhr.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
    xhr.send(JSON.stringify(value));
}

document.addEventListener('DOMContentLoaded', function() {
    const form = document.querySelector('form[name="change-address-form"');
    form.addEventListener('submit', handleSubmit);
});
```

```javascript
if (this.readyState == 4) {
            const responseJson = JSON.parse(this.responseText); // parsea también prototipos
            const form = document.querySelector('form[name="change-address-form"');
            const formParent = form.parentElement;
            form.remove();
            const div = document.createElement("div");
            if (this.status == 200) {
                const header = document.createElement("h3");
                header.textContent = 'Updated Billing and Delivery Address';
                div.appendChild(header);
                formParent.appendChild(div);
                for (const [key, value] of Object.entries(responseJson).filter(e => e[0] !== 'isAdmin')) { //Itera
                    const label = document.createElement("label");
                    label.textContent = `${toLabel(key)}`;
                    div.appendChild(label);
                    const p = document.createElement("p");
                    p.textContent = `${JSON.stringify(value).replaceAll("\"", "")}`;
                    div.appendChild(p);
                }
```

Esto sería un mass asigment más que otra cosa, asi que no tiene sentido :

![[Pasted image 20250429114844.png]]

El valor de `isAdmin` **no se establece explícitamente en ningún lugar**. Esto implica que:

- Si el formulario (`change-address-form`) no tiene un campo llamado `isAdmin`, entonces no formará parte del objeto `value` que se envía al servidor.
    
- En el **objeto de respuesta JSON** que se recibe del servidor (`responseJson`), sí podría haber una clave `isAdmin`, pero el código filtra esa entrada con `.filter(e => e[0] !== 'isAdmin')` para **no mostrarla en el resultado renderizado en la página**.
    

### Entonces, ¿qué valor tiene `isAdmin` por defecto?

- **En el cliente (JavaScript):** No se le asigna ningún valor, por lo que no existe a menos que el servidor lo incluya en la respuesta.
    
- **En el servidor:** Depende completamente de la implementación del backend. Si el servidor espera o genera un valor `isAdmin`, este podría ser `false`, `true`, `null`, o estar ausente, dependiendo de la lógica del backend.

De alguna forma algún código en el backend está verificando si existe isAdmin en Object.prototype y si existe lo añade, sino lo pone por defecto como false.

```json
{
	"address_line_1":"Wiener HQ",
	"address_line_2":"One Wiener Way",
	"city":"Wienerville",
	"postcode":"BU1 1RP",
	"country":"UK",
	"sessionId":"Ewgea1uJsOBBJUCUkKoAsbFEPRvkldLZ",
	"__proto__": {
	        "isAdmin":true
	    }
	}
```