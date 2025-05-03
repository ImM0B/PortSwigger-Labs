
Las configuraciones que se suelen usar : 
```html
<head>
	<style>
		#target_website {
			position:relative;
			width:128px;
			height:128px;
			opacity:0.00001;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			width:300px;
			height:400px;
			z-index:1;
			}
	</style>
</head>
```

Tenemos que usar clickbandit para efectuar el ataque , la idea es copiar el script y pegarlo en la consola del sitio:

![[{9E3ED6C8-6690-4D50-AC6D-2313712D4540}.png]]

Accedemos a :
`https://0aac005e03749063827d513b00f6007a.web-security-academy.net/my-account`

Grabamos las acciones y pegamos el html en el exploit server