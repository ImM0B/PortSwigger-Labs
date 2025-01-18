https://portswigger.net/web-security/sql-injection/cheat-sheet

On Oracle databases, every `SELECT` statement must specify a table to select `FROM`. If your `UNION SELECT` attack does not query from a table, you will still need to include the `FROM` keyword followed by a valid table name.

There is a built-in table on Oracle called `dual` which you can use for this purpose. For example: `UNION SELECT 'abc' FROM dual`

![[Pasted image 20241222230510.png]]
![[Pasted image 20241222230540.png]]
![[Pasted image 20241222231001.png]]

```SQL
'+UNION+SELECT+BANNER,'a'+FROM+v$version--
```


Cuando intentas una inyección con `UNION`, necesitas que el número de columnas en tu consulta inyectada coincida con el número de columnas de la consulta original del servidor. Esto se debe a que el operador `UNION` combina los resultados de dos consultas, y ambas deben tener:

1. **El mismo número de columnas**.
2. **Tipos de datos compatibles** en cada columna correspondiente.