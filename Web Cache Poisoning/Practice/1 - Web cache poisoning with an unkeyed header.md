1. Clic derecho en la request de la home page -> Extensions/Param Miner/Guest Headers/Ok
2. Extensions/Installed/Param Miner/Output

### ¿Qué hace `X-Forwarded-Host`?

`X-Forwarded-Host` envía el valor del host original (o dominio) de una solicitud HTTP que ha pasado por un proxy. Este encabezado es particularmente útil cuando el proxy cambia el valor de la URL o el dominio antes de reenviar la solicitud al servidor de destino. El servidor puede usar este encabezado para saber cuál era el dominio original que solicitó el usuario.

Por ejemplo, imagina una solicitud que pasa a través de un proxy y llega al servidor final:
- Solicitud original: `https://ejemplo.com/pagina`
- Host después de pasar por el proxy: `https://proxy.ejemplo.com/pagina`

Si el proxy envía la solicitud al servidor final con el encabezado `X-Forwarded-Host: ejemplo.com`, el servidor final puede entender que la solicitud original estaba destinada a `ejemplo.com`, no al proxy.

![[Pasted image 20241105170304.png]]
![[Pasted image 20241105170344.png]]
